---
title: Android-Jetpack架构之Databinding
date: 2018-09-19 19:50:38
categories: Android
tags: 
	- Android
	- Jetpack
	- Databinding
---

在AndroidStudio中使用Databinding需要你在`build.gradle`中配置启用

	android {
		.....
		dataBinding {
        	enabled = true
    	}
	}
	    

<!-- More -->
## [概述](https://developer.android.google.cn/topic/libraries/data-binding/)

Databinding是谷歌提供的一套支持库，可以让你将数据绑定到布局文件中。
在传统中，我们将数据绑定到UI组件中都是在代码中调用组件的方法来更新，例如下面我们通过`findViewById`来找到`TextView`,
将`viewModel`的`userName`属性绑定到`TextView`上

	finbViewById<TextView>(R.id.tv_user_name).apply {
		text = viewModel.userName
	}



## 初识


## 在布局中绑定


## UI数据同步变化

## 在代码中使用Databinding

## 在Adapter中应用Databinding

## 与其他架构组件的使用

## 多种绑定方式








