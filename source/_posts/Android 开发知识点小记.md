---
title: Android 开发知识点小记
date: 2018-09-19 19:50:38
categories: Android
tags: 
    - Android
---

记录Android开发过程中一些开发问题，持续更新！

<!-- more -->

## 字符串使用TextUtil工具类

拼接字符串

	TextUtils.join(",", cookies.keySet()));

分割字符串

	TextUtils.split()
	
可以替代"" + "" + "	

	TextUtils.concat(CharSequence... text); 
	
可以编码html中的< >等字符为&lt;

	TextUtils.htmlEncode()
	


## 字符串拼接使用字符串方法concat

	String str = ""; str.concat("ccc");

参考阅读：[http://blog.csdn.net/youanyyou/article/details/78992978](http://blog.csdn.net/youanyyou/article/details/78992978 "字符串拼接+和concat的区别")

## 查看工程依赖的依赖库

1. View->ToolWindow->Gradle
2. 选择对应项目或者Module中Tasks->android, 双击androidDependencies
3. 打开Gradle Console就可以查看依赖关系了

## 发送短信

  	Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("sms:" + phoneNum));
    intent.putExtra("sms_body", message);
	startActivity(intent);

这里需要特别注意  Uri.parse(***"sms:"*** + phoneNum) 必须是**sms** 而不是网上说的 **smsto**

## adb命令

#### 打印Activity栈信息

	adb shell dumpsys activity | [grep package-name| grep mFocusedActivity]

## 获取系统actionbar高度

	int atts = new int[]{android.R.attr.actionBarSize};
    TypedArray ta = context.obtainStyledAttributes(attrs);
    float actionBarHeight = ta.getDimension(0, 0);
    ta.recycle();
    return actionBarHeight;

## 拦截父布局Touch事件

在需要的时候调用

	getParent().requestDisallowInterceptTouchEvent(false);


## getDimension和getDimensionPixelOffset的区别

getDimension和getDimensionPixelOffset的功能类似，

都是获取某个dimen的值，但是如果单位是dp或sp，则需要将其乘以density

如果是px，则不乘。并且getDimension返回float，getDimensionPixelOffset返回int.

而getDimensionPixelSize则不管写的是dp还是sp还是px,都会乘以densti

## 8.0弹通知栏

8.0以上的系统对通知栏做了处理，弹通知栏必须增加channel_id

以前我们这样设置

    Notification notify = new Notification.Builder(context, )
            .setAutoCancel(true)
            //设置通知栏图标
            .setSmallIcon(R.drawable.title_icon)
            .setLargeIcon(BitmapFactory.decodeResource(context.getResources(), R.drawable.title_icon))
            //设置通知内容的标题
            .setContentTitle(title)
            //设置通知内容
            .setContentText(content)
            //或者自定义声音
            //setsound(uri.parse())
            //设置要启动的程序
            .setContentIntent(pi)
            .setSmallIcon(R.drawable.title_icon)
            //最后用build来建立通知
            .build();

8.0以后需要增加

    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
        // Create the NotificationChannel
        CharSequence name = "china mobile for hebei";
        String description = "hebei mobile heshenghuo";
        int importance = NotificationManager.IMPORTANCE_DEFAULT;
        NotificationChannel mChannel = new NotificationChannel("my_channel_id", name, importance);
        mChannel.setDescription(description);
        // Register the channel with the system; you can't change the importance
        // or other notification behaviors after this
        NotificationManager notificationManager = (NotificationManager) context.getSystemService(Context.NOTIFICATION_SERVICE);
        notificationManager.createNotificationChannel(mChannel);
    }
同时创建Buidler时候需要传入channel_id  **new NotificationCompat.Builder(context, "my_channel_id")**

参考：[https://developer.android.google.cn/training/notify-user/channels.html](https://developer.android.google.cn/training/notify-user/channels.html "https://developer.android.google.cn/training/notify-user/channels.html")

## 设置PopupWindow背景延伸到状态栏

	popupWindow.setClippingEnabled(false);
该属性的设置可以将PopupWindow全屏显示，否则会只展示到状态栏一下

## Fragment中onActivityResult无法调用

[https://www.jianshu.com/p/ca91fa528d5c](https://www.jianshu.com/p/ca91fa528d5c)

## 竖屏的Activity切换到横屏的Activity后，点击返回报错

我首页竖屏的Activity A这样配置的

    <activity
        android:name=".ui.main.MainActivity"
        android:launchMode="singleTask"
       
        android:screenOrientation="portrait" />

横屏的Activity B

    <!-- 证件识别 -->
    <activity
        android:name=".ui.detect.DetectActivity"
        android:screenOrientation="landscape"
        android:theme="@style/DetectCaptureTheme" />

在A进入B后，B点击返回就崩了，
[https://blog.csdn.net/qq_35353487/article/details/79035744](https://blog.csdn.net/qq_35353487/article/details/79035744)

这个博客找到答案  **android:configChanges="orientation|screenSize|keyboardHidden"**
在Maintivity 中配置 **android:configChanges="orientation|screenSize|keyboardHidden"**，这样在屏幕旋转不会重新走生命周期

所以根本问题是横竖屏的Activity在切换时候生命周期重建了

## Android 渗透测试

### 使用工具：Drozer

### 安装：[https://www.cnblogs.com/JDragons/p/5596258.html](https://www.cnblogs.com/JDragons/p/5596258.html "https://www.cnblogs.com/JDragons/p/5596258.html")

### 基本使用：[https://www.cnblogs.com/goodhacker/p/3906180.html](https://www.cnblogs.com/goodhacker/p/3906180.html "https://www.cnblogs.com/goodhacker/p/3906180.html")

### 命令：[https://blog.csdn.net/cch139745/article/details/53519900](https://blog.csdn.net/cch139745/article/details/53519900 "https://blog.csdn.net/cch139745/article/details/53519900")


### 基本命令

* 查看所有包

>  **run app.package.list** 
>  
>  [-f xxx 查找匹配xxx的包]

* 查看包信息

>  **run app.package.info** 
>  
>  -a [package-name]

* 启动Activity

>  **run app.activity.start** 
>  
>  [-- component xx.xxx.xxActivity] 
>  
>  [--flags ACTIVITY_NEW_TASK] 
>  
>  [--action android.intent.action.VIEW] 
>  
>  [--data-uri http://www.google.com]

* 获取Activity信息

> **run app.activity.info**
> 
> [-a package-name]
> 
> [-f activity-name]
> 
> [-u, –unexported]
> 
> [-v, –verbose]

## BroadcastReceiver 中系统Action

> **android.intent.action.USER_PRESENT**  监听屏幕解锁的Action


## JobService

android  在21以后使用JobService, 兼容方案：https://github.com/firebase/firebase-jobdispatcher-android


## Android 9.0行为变更小计

参考：[https://developer.android.google.cn/preview/behavior-changes](https://developer.android.google.cn/preview/behavior-changes "https://developer.android.google.cn/preview/behavior-changes")

#### 强制执行 FLAG_ACTIVITY_NEW_TASK 要求
在 Android P 中，您不能从非 Activity 环境中启动 Activity，除非您传递 Intent 标志 FLAG_ACTIVITY_NEW_TASK。 如果您尝试在不传递此标志的情况下启动 Activity，则该 Activity 不会启动，系统会在日志中输出一则消息。

注：在 Android N 之前，标志要求一直是期望的行为并被强制执行。 Android N 中的一个错误会临时阻止实施标志要求。

#### 前台服务
针对 Android P 或更高版本并使用前台服务的应用必须请求 FOREGROUND_SERVICE 权限。 这是普通权限，因此，系统会自动为请求权限的应用授予此权限。

如果针对 Android P 的应用尝试创建一个前台服务且未请求 FOREGROUND_SERVICE，则系统会引发 SecurityException。

## Android 8.0 行为变更小计

#### 后台执行限制
Android 8.0 为提高电池续航时间而引入的变更之一是，当您的应用进入已缓存状态时，如果没有活动的组件，系统将解除应用具有的所有唤醒锁。

此外，为提高设备性能，系统会限制未在前台运行的应用的某些行为。具体而言：

现在，在后台运行的应用对后台服务的访问受到限制。
应用无法使用其清单注册大部分隐式广播（即，并非专门针对此应用的广播）。
默认情况下，这些限制仅适用于针对 O 的应用。不过，用户可以从 Settings 屏幕为任意应用启用这些限制，即使应用并不是以 O 为目标平台。

Android 8.0 还对特定函数做出了以下变更：

* 如果针对 Android 8.0 的应用尝试在不允许其创建后台服务的情况下使用 startService() 函数，则该函数将引发一个 IllegalStateException。
* 新的 Context.startForegroundService() 函数将启动一个前台服务。现在，即使应用在后台运行，系统也允许其调用 Context.startForegroundService()。不过，应用必须在创建服务后的五秒内调用该服务的 startForeground() 函数。
 

#### 应用快捷键
Android 8.0 对应用快捷方式做出了以下变更：

* com.android.launcher.action.INSTALL_SHORTCUT 广播不再会对您的应用有任何影响，因为它现在是私有的隐式广播。相反，您应使用 ShortcutManager 类中的 requestPinShortcut() 函数创建应用快捷方式。
* 现在，ACTION_CREATE_SHORTCUT Intent 可以创建可使用 ShortcutManager 类进行管理的应用快捷方式。此 Intent 还可以创建不与 ShortcutManager 交互的旧版启动器快捷方式。在以前，此 Intent 只能创建旧版启动器快捷方式。
* 现在，使用 requestPinShortcut() 创建的快捷方式和在处理 ACTION_CREATE_SHORTCUT Intent 的操作组件中创建的快捷方式均已转换为功能齐全的应用快捷方式。因此，应用现在可以使用 ShortcutManager 中的函数来更新这些快捷方式。
* 旧版快捷方式仍然保留了它们在旧版 Android 中的功能，但您必须在应用中手动将它们转换成应用快捷方式。



如需了解有关应用快捷方式变更的更多信息，请参阅固定快捷方式和微件预览功能指南。

#### 网页表单自动填充
现在，Android 自动填充框架提供对自动填充功能的内置支持，对于安装到运行 Android 8.0 的设备上的应用，与 WebView 对象相关的下列函数已经发生变化：

**WebSettings**
* getSaveFormData() 函数现在返回 false。之前，此函数返回 true。
* 调用 setSaveFormData() 不再有任何效果。
**WebViewDatabase**
* 调用 clearFormData() 不再有任何效果。
* hasFormData() 函数现在返回 false。之前，当表单包含数据时，此函数返回 true。

## 集合的处理
现在，AbstractCollection.removeAll() 和 AbstractCollection.retainAll() 始终引发 NullPointerException；之前，当集合为空时不会引发 NullPointerException。此项变更使行为符合文档要求。

## 大神博客


进行系统级别的Android源码分析
[http://gityuan.com/page2/](http://gityuan.com/page2/ "http://gityuan.com/page2/")

## Android 默认的插值动画

	AccelerateDecelerateInterpolator 在动画开始与结束的地方速率改变比较慢，在中间的时候加速
	AccelerateInterpolator  在动画开始的地方速率改变比较慢，然后开始加速
	AnticipateInterpolator 开始的时候向后然后向前甩
	AnticipateOvershootInterpolator 开始的时候向后然后向前甩一定值后返回最后的值
	BounceInterpolator   动画结束的时候弹起
	CycleInterpolator 动画循环播放特定的次数，速率改变沿着正弦曲线
	DecelerateInterpolator 在动画开始的地方快然后慢
	LinearInterpolator   以常量速率改变
	OvershootInterpolator    向前甩一定值后再回到原来位置

## Andorid 动画详解

[Android动画](https://www.cnblogs.com/ldq2016/p/5407061.html "Android动画")

## 模拟器端口转发命令
    adb forward tcp:9999 tcp:9999

## AsyncHttpClient增加对https支持

项目网络框架使用的是android-async-http，不过现在基本很少人使用了，服务器接入了https，所以客户端也需要配置。可以通过`AsyncHttpClient`的`setSSLSocketFactory`方法设置。该方法需要传入一个`SSLSocketFactory`类型的对象。通过该对象的源码看到可以通过多种方式创建。如果是双向验证可以使用

```
    public SSLSocketFactory(
            final KeyStore keystore,
            final String keystorePassword,
            final KeyStore truststore)
                throws NoSuchAlgorithmException, KeyManagementException, KeyStoreException, UnrecoverableKeyException {
        this(SSLContexts.custom()
                .loadKeyMaterial(keystore, keystorePassword != null ? keystorePassword.toCharArray() : null)
                .loadTrustMaterial(truststore)
                .build(),
                BROWSER_COMPATIBLE_HOSTNAME_VERIFIER);
    }
```
keysore是服务器信任客户的使用的，需要使用到.p12文件。
keystorePassword是对应keystore的密码，
truststore是信任服务器证书 需要使用.bks文件

如果是只信任服务器的证书可以使用`SSLSocketFactory`的子类`MySSLSocketFactory`，首先我们需要先生成.bks文件，参考下面链接
[https://blog.csdn.net/qq_33237207/article/details/53376232](https://blog.csdn.net/qq_33237207/article/details/53376232)
如果你使用keytool命令无效，可以切换到keytool目录下(JDK_HOME\jre\bin),然后执行就可以。执行命令需要输入密钥这个密钥下面代码会使用到

```
C:\Program Files\Java\jdk1.8.0_191\jre\bin>keytool -importcert -keystore C:\al.b
ks -file C:\cert.crt -storetype BKS -provider org.bouncycastle.jce.provider.Boun
cyCastleProvider
输入密钥库口令:
再次输入新口令:
所有者: CN=t110, OU=ss, O=, L=shijiazhuang, ST=HB, C=CN
发布者: CN=t110, OU=ss, O=, L=shijiazhuang, ST=HB, C=CN
序列号: ce9c305d3c167fb9
有效期为 Fri Jun 29 15:02:31 CST 2018 至 Sat Jun 29 15:02:31 CST 2019
证书指纹:
         MD5:  D7:55:11:40:14:F2:03:54:29:6A:F8:F4:30:C1:EC:3C
         SHA1: B4:E8:C7:C2:AF:2E:BF:B0:D1:32:76:16:58:FA:62:28:09:C4:79:39
         SHA256: 93:D6:FA:B4:03:0D:DC:A0:D6:23:66:F2:11:D7:83:1F:7C:45:4B:E8:B8:
14:80:5C:7A:20:8C:7E:B4:A9:67:C8
签名算法名称: SHA1withRSA
主体公共密钥算法: 1024 位 RSA 密钥
版本: 1
是否信任此证书? [否]:  y
证书已添加到密钥库中
```

将生成的bks文件拷贝到工程的`res\raw`目录下,配置如下代码：

```


private static MySSLSocketFactory getSocketFactory(Context context) {
    MySSLSocketFactory sslFactory = null;
    try {
        KeyStore keyStore = KeyStore.getInstance("bks");
        InputStream isCert = context.getResources().openRawResource(R.raw.al);

        // HTTPS_PWD就是上面创建bks文件时候输入的密钥
        keyStore.load(isCert, HTTPS_PWD.toCharArray());

        sslFactory = new MySSLSocketFactory(keyStore);
        // 下面的设置是解决javax.net.ssl.SSLException: hostname in certificate didn't match: 配置后其实是跳过了https的验证，不配值签名证书也可以访问
        sslFactory.setHostnameVerifier(new AllowAllHostnameVerifier());
    } catch (KeyStoreException e1) {
        e1.printStackTrace();
    } catch (NoSuchAlgorithmException e1) {
        e1.printStackTrace();
    } catch (CertificateException e1) {
        e1.printStackTrace();
    } catch (IOException e1) {
        e1.printStackTrace();
    } catch (UnrecoverableKeyException e1) {
        e1.printStackTrace();
    } catch (KeyManagementException e) {
        e.printStackTrace();
    }
    return sslFactory;
}

...
MySSLSocketFactory factory = getSocketFactory(context);
if (factory != null) {
    client.setSSLSocketFactory(factory);
}

```
[相关链接](https://www.jianshu.com/p/64172ccfb73b)

## AutoFill

简单说就是实现表单自动提示，用户可以选择填写内容

[Google官方Demo](https://github.com/googlesamples/android-AutofillFramework)
[介绍](https://blog.csdn.net/ArJinMC/article/details/73331022)