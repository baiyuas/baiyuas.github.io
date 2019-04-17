---
title: OkHttp源码鸡西
date: 2018-03-07 16:46:38
categories: Android
tags:
	- OkHttp
---

OkHttp 基于Socket封装的网络框架，[Github地址](https://github.com/square/okhttp)

对于Socket的操作都是基于[Okio库](https://github.com/square/okio)
Okio主要两个接口 Sink和Source
Sink是对byte的写操作，
Source是对byte的读操作

我们创建一个请求首先是构建一个Request，通过Request内部类Builder可以传入需要配置的参数
如果是Post请求还需要传入RequestBody,示例如下：

```
RequestBody requestBody = new FormBody.Builder()
            .add("key1", "value1")
            .add("key1", "value2")
            .build();
Request request = new Request.Builder()
        .url(url)
        .addHeader("headerKey1", "headerValue1")
        .addHeader("headerKey2", "headerValue2")
        .post(requestBody)
        .build();
```

之后可以通过OkHttpClient发送请求，而OkHttpClient也是通过OkHttpClient.Builder来传入配置信息构造而成

```
OkHttpClient client = new OkHttpClient.Builder()
        .connectTimeout()
        .build();
Response response = client.newCall(request).execute();
```
然后我们j就可以获取Response来获取响应的数据， 从newCall方法进入查看网络请求过程。

```
  /**
   * Prepares the {@code request} to be executed at some point in the future.
   */
  @Override public Call newCall(Request request) {
    return RealCall.newRealCall(this, request, false /* for web socket */);
  }

static RealCall newRealCall(OkHttpClient client, Request originalRequest, boolean forWebSocket) {
    // Safely publish the Call instance to the EventListener.
    RealCall call = new RealCall(client, originalRequest, forWebSocket);
    call.transmitter = new Transmitter(client, call);
    return call;
  }
```

newCall方法创建一个了RealCall,传入了Request对象，创建RealCall实例化一个对象**Transmitter**该对象是应用层和网络程的桥梁，提供整个请求过程使用的connections, requests, responses, and streams。

```
 @Override public Response execute() throws IOException {
 	// 1
    synchronized (this) {
      if (executed) throw new IllegalStateException("Already Executed");
      executed = true;
    }
    // 2
    transmitter.timeoutEnter();
    // 3
    transmitter.callStart();
    try {
    	// 4
      client.dispatcher().executed(this);
      // 5
      return getResponseWithInterceptorChain();
    } finally {
    	// 6
      client.dispatcher().finished(this);
    }
  }
```
获取RealCall之后调用了execute方法。这里调用execute是同步执行，如果是异步执行使用equence方法，请求核心部分是相同的。execute方法 //1处可以看到使用了同步锁，线程安全。如果正在执行则放弃。
//2 timeoutEnter()内部主要是将重置Socket，断开已链的请求
//3 回调接口EventListener通知开始执行请求，EventListener在创建OkHttpClient时候可以通过OkHttpClient.Builder传入
//4 client.dispatcher()实际是一个OkHttp内部的线程池Dispatcher，调用.executed(this);将当前RealCall加入到线程池等待执行。RealCall实现了Call接口
//5 获取拦截器链，这也是OkHttp的核心请求部分，下面也会对此展开分析，传说这个叫做责任链模式
//6 将当前请求移除线程池

下面来分析下拦截器链的部分，先看下代码

```
Response getResponseWithInterceptorChain() throws IOException {
    // Build a full stack of interceptors.
    List<Interceptor> interceptors = new ArrayList<>();
    interceptors.addAll(client.interceptors());
    interceptors.add(new RetryAndFollowUpInterceptor(client));
    interceptors.add(new BridgeInterceptor(client.cookieJar()));
    interceptors.add(new CacheInterceptor(client.internalCache()));
    interceptors.add(new ConnectInterceptor(client));
    if (!forWebSocket) {
      interceptors.addAll(client.networkInterceptors());
    }
    interceptors.add(new CallServerInterceptor(forWebSocket));

    Interceptor.Chain chain = new RealInterceptorChain(interceptors, transmitter, null, 0,
        originalRequest, this, client.connectTimeoutMillis(),
        client.readTimeoutMillis(), client.writeTimeoutMillis());

    boolean calledNoMoreExchanges = false;
    try {
      Response response = chain.proceed(originalRequest);
	......
```

