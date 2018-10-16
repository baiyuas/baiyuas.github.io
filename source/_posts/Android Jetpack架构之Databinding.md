---
title: Android-Jetpack架构之Databinding
date: 2018-09-19 19:50:38
categories: Android
tags: 
	- Android
	- Jetpack
	- Databinding
---  


Databinding是谷歌提供的一套支持库，可以让你将数据绑定到布局文件中。对于这个高大上的东西我觉得还是学习官方文档更加有效，而且官方文档讲解的也比较详细，虽然有中文版本，但是感觉还是自己翻译学习比较有效果。所以有了这边译文。

后续我会将Jetpack系列框架都翻译学习下。主要包含

* DataBinding
* Lifecycles
* LiveData
* Navigation
* Paging Library
* Room
* ViewModel
* WorkManager


<!-- More -->

## [概述](https://developer.android.google.cn/topic/libraries/data-binding/)

在传统中，我们将数据绑定到UI组件中都是在代码中调用组件的方法来更新，例如下面我们通过`findViewById`来找到`TextView`,
将`viewModel`的`userName`属性绑定到`TextView`上

```
	finbViewById<TextView>(R.id.tv_user_name).apply {
		text = viewModel.userName
	}
```

下面这个段代码示例展示了如何使用Data Binding库来应用文本内容到布局文件的组件上，注意赋值表达式中`@{}`的使用

```
<TextView 
	android:text="@{viewModel.userName}"
```

在布局文件中绑定组件可以让你减少Activity中UI框架的调用，使代码更加简单。同时也可以提高你应用的性能，有效避免内存泄漏和空指针的出现。

下面将会学习如何在你的应用中使用Data Binding库。可以看示例代码[Android Data Binding Library samples](https://github.com/googlesamples/android-databinding)

## 初识

学习如何获取开发环境来准备使用Data Binding库，包含在Android Studio中使用Data Binding代码支持。

Data Binding库作为一个支持库提供了灵活和广泛的兼容性，因此你可以用它运行在Android4.0以上的设备。

推荐使用最新的Android Gradle插件，Data Binding需要Android Gradle插件1.5以上版本，更多信息参考[如何升级Android Gradle插件](https://developer.android.google.cn/studio/releases/gradle-plugin#updating-plugin)

### 编译环境

在AndroidStudio中使用Databinding需要你在`build.gradle`中配置启用

```
	android {
		.....
		dataBinding {
        	enabled = true
    	}
	}
```
### Android Studio对Data Binding的支持

Android Studio支持许多对Data Binding代码编辑的支持。例如支持以下Data Binding表达式的支持

* 语法高亮
* 表达式语法错误标记
* XML 代码自动完成
* 快速导航

在预览页面展示一个编辑时候的默认值。例如下面代码在预览页面将会展示`my_default`在这个`TextView`上

```
<TextView android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	android:text="@{user.firstName, default=my_defalut}"
```

### 新的Data Binding编译器

在3.1.0-alpha06以上的版本的Android Gradle插件中包含了最新的DataBingding编译器，能更好的减少编译生成绑定类失败引起错误提示。旧版本Android Gradle插件可以在`gradle.properties`中配置来开启新版本的编译器

	android.databinding.enableV2=true

新版本Databind编译器具有如下行为变更:

* 在你编译代码之前Android Gradle插件就会生成绑定类
* 如果一个布局被包含在不只一个目标资源配置里，databingding将会用`android.view.View`作为具有相同ID但是view类型不同的所有View默认的View类型
* 生成的绑定类对于当前的modules可以打包到aar文件里。其他依赖这个modlues的modlues不会生成绑定类
* 对于Adapter绑定对于依赖他的modlues事无效的，它对自己的modlues有作用

## 在布局中绑定

你可以在布局中使用表达式语言来处理视图事件。在布局文件中用你的数据对象绑定的views,这个Databinding库将会自动对应的类

使用Data binding的布局文件与其他布局文件略有不同，它是用`layout`作为根标签，下面包含一个`data`和`view`根元素，而这个view根元素就是非Databinding布局文件中的根标签。下面示例展示了Databinding布局文件

```
<?xml version="1.0" encoding="utf-8"
<layout xmlns:android="http://schemas.android.com/apk/res/android">
	<data>
		<variable name="user" type="com.example.User" />
	</data>
	<LinearLayout
		android:orientation="vertical"
		android:layout_width="match_parent"
		android:layout_height="match_parent">

		<TextView andriod:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:text="@{user.firstName}" />

		<TextView andriod:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:text="@{user.lastName}" />
	</LinearLayout>
```

其中在`data`标签下的`user`变量描述了可以在布局文件中使用的属性

	<variable name="user" type="com.example.User" />

在布局文件中使用表达式`@{}`的方式将变量的值赋予到组件中去。这里`TextView`设置了变量`user`的`firstName`的属性值

	<TextView android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:text="@{user.firstName}" />

### 数据对象

我们先创建一个简单的对象`User`

	data class User(val firstName:String, val lastName:String)

这中`data`对象的数据是不可变的，通常只读取一次之后不可更改，同时遵循类似Java中的方法访问的规则。下面是Java中等效的代码：

```
public class User {
	private String firstName;
	private Strign lastName;

	public User(String firstName, String lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
	}

	public String getFristName() {
		return this.firstName;
	}

	public String getLastName() {
		return this.lastName;
	}

}
```

从Databinding的角度而言，两个类是等效的。例如在`android:text`属性中使用的`@{user.firstName}`这个表达式所访问的`firstName`属性值是与`getFirstName()`方法

### 绑定数据

为每个`databinding`布局文件生成的对象，默认是使用布局文件名称按着帕斯卡命名规范每个字首字母大写。添加Binding作为后缀而生成的名称。例如`activity_main.xml`的布局文件会生成名为`ActivityMainBinding`的类，这个类持有所有布局中`views`，同时给binding表达式赋值。推荐在解析布局文件时候来创建`binding`,如下示例

```
override fun onCreate(savedInstantceState: Bundle?) {
	super.onCreate(savedInstantceState);

	val binding: ActivityMainBinding = DataBindingUtil.setContentView(this, R.layout.activity_main);
	binding.user = User("Test", "User")
}
```
运行后会在UI界面中展示`Test`用户，你也可以像下面这样获取view通过`LayoutInflater`

	val binding: ActivityMainBinding = ActivityMainBinding.inflate(getLayoutInflater())

*ps:这里在翻译官方文档时候，没有理解什么意思，我按着这个代码获取布局的binding后，给设置值，页面没有显示结果。*

如果你使用Databinding在`ListView`，`Fragment`或者`RecyclerView`适配器中，你可以使用`DataBindingUtil`的`inflate()`方法，如下：

```
val listItemBinding = DataBindingUtil.inflate(layoutInflater, laoutId, viweGroup, false)
```

### 表达式

这表达式类似代码中的表达式，你可以使用下面的操作符和关键字在布局中

* 数学运算符 `+ - / * %`
* 字符串拼接 `+`
* 逻辑运算符 `|| && `
* 二进制运算符 `| & ^`
* 一元运算 `+ - ! ~`
* 移位 `>> >>> <<`
* 比较 `== > < >= <=`
* 类型推断 `instantceof`
* 组 `()`
* 三目运算 `? =`
* 数组访问 `[]`
* 对象中方法的调用
* 对象中字段的访问

例如：

```
android:text="@{String.valueOf(index + 1)}"
android:visibility="@{age < 13 ? View.GONE : View.VISIBLE}"
android:transitionName='@{"image_" + id}'
```

#### 空判断操作符

空判断操作符`??`判断如果左边非空使用左边值，如果为空则选择右边值

	android:text="@{user.dispalyName ?? user.lastName}"

与下面是等效

	android:text="@{user.dispalyName == null ? user.lastName " user.dispalyName"}"	

Databinding在生成代码时候会字段进行`null`判断，避免空指针异常的发生。例如，在表达式`@{user.name}`中，如果`user`是空，则`user.name`会分配默认值或者`null`字符串，如果是数字则默认会是0

#### 集合

通常的集合，例如数组，列表和maps都可以通过`[]`操作符来访问

```
<data>
	<import type="android.util.SparseArray" />
	<import type="java.util.Map" />
	<import type="java.tuil.List" />

	<variable name="list" type="List<String>" />
	<variable name="sparse" type="SparseArray<String>" />
	<variable name="map" type="Map<String, String>" />
	<variable name="index" type="int" />
	<variable name="key" type="String" />
</data>

...
android:text="@{list[index]}"
...
android:text="@{sparse[index]}"
...
android:text="@{map[key]}"
```

> ☆ 注：你也可以使用**@{map.key}**替换**@{map[key]}**

> 另外这里需要注意新编译器 *`<variable name="list" type="List<String>" />`*里面的`List<String>`需要使用`List&lt;String&gt;`替换否则会报错

#### 在集合迭代器中字符串使用

	android:text='@{map["firstName"]'

如上你可以使用单引号在外侧，内侧使用双引号，你可以外侧使用双引号，但是内侧可不是单引号，而是反向单引号  **\`**，如下:

	android:text="@{map[`firstName`]}"

#### 表达式对于资源文件的引用

Databinding在布局文件中表达式也可以使用布局文件如下：

	android:padding="@{large ? @dimen/largePadding : @dimen/smallPadding}"

在`res/values/dimens.xml`中

```
<dimen name="largePadding">8dp</dimen>
<dimen name="smallPadding">4dp</dimen>
```

也可以通过参数的方式来格式化字符串和复数

```
android:text="@{@string/nameFormat(firstName, lastName)"
android:text="@{@plurals/banana(count)}"
```

如果一个plural中有多个参数，则多个参数都要肤质

```
  Have an orange
  Have %d oranges

android:text="@{@plurals/orange(count, count)}"
```
*关于`Plurals`参考官方介绍[Plurals](https://developer.android.google.cn/guide/topics/resources/string-resource#Plurals)*

下表是一些资源文件和表达式中引用对照表

| Type        | 正常引用   |  表达式中引用  |
| --------   | -----:  | :----:  |
| String[]     | @array |   @stringArray     |
| int[]        | @array |   @intArray    	 |
| TypedArray[]        | @array |   @typedArray    	 |
| Animator        | @animator |   @animator    	 |
| StateListAnimator        | @animator |   	@stateListAnimator    	 |
| color int        | @color |   @color    	 |
| ColorStateList        | @color |   @colorStateList    	 |

#### 事件处理



## UI数据同步变化

## 在代码中使用Databinding

## 在Adapter中应用Databinding

## 与其他架构组件的使用

## 多种绑定方式








