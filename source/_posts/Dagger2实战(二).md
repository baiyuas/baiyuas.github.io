---
title: Dagger2实战(二)
date: 2018-03-07 16:46:38
categories: Android
tags:
	- Dagger
---

本篇主要介绍下Dagger的Android扩展库的实战，官方的Todo-mvp-dagger示例介绍了如何在应用中使用dagger-android，但是都是集成DaggerAppliction或者DaggerActivity等，我不喜欢，这里介绍集成Appliction时候使用方法，

<!-- more -->

依赖库

    implementation "com.google.dagger:dagger:${libs.dagger}"
    annotationProcessor "com.google.dagger:dagger-compiler:${libs.dagger}"
    provided 'org.glassfish:javax.annotation:10.0-b28'
    compile "com.google.dagger:dagger-android:${libs.dagger}"
    compile "com.google.dagger:dagger-android-support:${libs.dagger}"
    annotationProcessor "com.google.dagger:dagger-android-processor:${libs.dagger}"

具体版本可以去github或者jcenter查看

首先需要创建一个容器
    
    @Singleton
    @Component(modules = {NetRepoModule.class,
    	AndroidBindingModule.class,
    	AndroidSupportInjectionModule.class})
    public interface AndroidAppComponent extends AndroidInjector<ByDaggerAndroidApp>{
    
    	void inject(Application application);
    }

这里AndroidSupportInjectionModule.class和AndroidInjector<ByDaggerAndroidApp>是必须的，具体根据自己Appliction配置，

然后在自己的Application需要实现HasActivityInjector接口
并且注入

    @Inject
    DispatchingAndroidInjector<Activity> activityInjector;

    @Override
    public AndroidInjector<Activity> activityInjector() {
        return activityInjector;
    }

同时在oncreate方法中注入

    @Override
    public void onCreate() {
        super.onCreate();
        DaggerAndroidAppComponent.create().inject(this);
    }

这样大框架基本构建完成

对于Activity依赖注入我们在AndroidBindingModule.class中来创建

	@Module
	public abstract class AndroidBindingModule {

    	@ActivityScope
    	@ContributesAndroidInjector(modules = {WelcomeModule.class})
    	abstract WelcomeActivity welcomeActivity();

	}

而这里WelcomeModule提供WelcomeActivti中一些使用的注入

    @Module
    public abstract class WelcomeModule {
    
        @ActivityScope
        @Binds
        abstract WelcomeContact.Presenter takePresenter(WelcomePresenter presenter);
    }

Activity中需要在oncreate中

    @Inject
    WelcomeContact.Presenter mWelcomePresenter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        AndroidInjection.inject(this);
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_welcome);
        mWelcomePresenter.reqFloorInfo();
    }

这样就完成了，有机会在讲解写扩展库的原理