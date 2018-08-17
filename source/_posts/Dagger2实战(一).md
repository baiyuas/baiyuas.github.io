---
title: Dagger2实战(一)
date: 2018-02-09 19:50:38
categories: Android
tags:
	- Dagger
---

Dagger是一个注入框架，更方便的管理类与类之间的关系，降低耦合。对于Dagger的说明，推荐这篇博文[Dagger 完全解析][1]，讲解的很详细。对于不了解Dagger的朋友很有帮助，我的这篇文章主要是我对一些注解的理解，方便快速使用Dagger。


## Dagger基本使用
Dagger核心的三个部分就是Compoment(容器)、Module(提供对象)、Inject(注入)，在Android中我们一般在Application中初始化容器，将需要的数据模型注入到容器中，在需要的地方通过Appliction获取容器之后就可以获取到数据。代码说明下：
我们先创建一个容器

    @Singleton
    @Component
    public interface AppComponent {
    
    }

比如我们需要网络数据，首先我们就可以定义一个接口

    public interface NetRepo {
    	List<FloorBean> fetchFloorInfo();
    }

创建实现类

    public class NetRepImpl implements NetRepo {
    	@Override
    	public List<FloorBean> fetchFloorInfo() {
    		return null;
    	}
    }

容器需要module来提供数据，所以我们创建一个Module

	@Module
	public class AppModule {

    	@Singleton
    	@Provides
    	NetRepo provideNetRepo() {
        	return new NetRepImpl();
    	}
	}

这个时候comoment就可以使用这个NetRepo对象并且对外提供

	@Singleton
	@Component(modules = {AppModule.class})
	public interface AppComponent {

    	NetRepo getNetRepo();
	}

我们编译一下工程就会自动生成DaggerAppCompoment.java类，我们依靠这个就可以获取NetRepo对象le。这就是一个Dagger简单的使用。有人会文这里没有看到Inject。因为在Module中我们直接`new`一个`NetRepImpl`，如果在方法`provideNetRepo()`我们修改为`provideNetRepo(NetRepImpl netRep)`，那我们就需要在`NetRepImpl`的构造上使用**@Inject**

    @Inject
    public NetRepImpl() {
    }

## 解惑**@Inject**

对于**@Inject**一开始时候我也很迷茫，到底都什么时候使用呢？我们举个例子说明下
比如我们想将B对象注入到A对象中
	
	class A {
		B b;
	}

情况一，我们在Module中提供来B对象



这时候我们需要在`A`对象的`b`属性使用**@Inject**




**情况一**，我们在Module中提供B对象，同时A构造中注入B，则A中b属性则不需要使用@Inject

	@Module
	public class AbModule {

		@Providers
		B provideB() {
			return new B();
		}
	}

	class A {
		private B b;
		public A(B b) {
			this.b = b;
		}
	}

**情况二**，我们在Module中没有提供B对象，但是A构造中注入了B，则A中b属性不需要使用@Inject，但是B的构造中需要使用@Inject

	@Module
	public class AbModule {

	}

	class A {
		private B b;
		public A(B b) {
			this.b = b;
		}
	}

	class B {
		@Inject
		public B {}
	}

**情况三**，我们在Module中提供B对象，A构造中不注入B对象，则A中b属性需要使用@Inject

	@Module
	public class AbModule {

		@Providers
		B provideB() {
			return new B();
		}
	}

	class A {
		@Inject
		private B b;
		public A() {}
	}

**情况四**，我们在Module中没有提供B对象，但是A构造中注入了B，则A中b属性需要使用@Inject，同时B的构造中需要使用@Inject

	@Module
	public class AbModule {
	}

	class A {
		@Inject
		private B b;
		public A() {}
	}

	class B {
		@Inject
		public B {}
	}

其他情况都是无法注入或者编译错误。








[1]: http://johnnyshieh.me/posts/dagger-advance/