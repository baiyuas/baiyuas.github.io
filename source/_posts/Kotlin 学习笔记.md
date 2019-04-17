---
title: 关于Kotlin的那些事
date: 2018-03-07 16:46:38
categories: Android
tags:
	- Kotlin
---

！[Kotlin Label](http://img5.imgtn.bdimg.com/it/u=241871401,855761362&fm=26&gp=0.jpg)
Kotlin学习过程中记录的笔记，部分内容会与Java做对比，更好的认识Kotlin

<!-- more -->

## 关于this

在Android中使用this 来指代Activity，在kotlin中要使用this@XXX

Java中
>  private HomeActivity context = this;

Kotlin中
>  var context: this@HomeActivity

## 关于变量

在Kotlin中定义变量都统一使用**var**关键字
> var 变量一: 类型


## 关于函数

函数是是哦那个fun关键字

无参无返回
	
	fun 函数名() {
	
	}

无参有返回值

	fun 函数名(): 返回类型 {

	}

有参有返回

	fun 函数名(): 返回类型{

	}

#### *扩展函数*

扩展函数就是对已有对象中增加函数，Android中可以扩展Context类，增加toast函数，这样在Activity里就可以直接使用toast函数

	fun Contex.toast(text: String) {
		Toast.makeText(this, text, Toast.SHORT_).show()
	}

在开源项目PoiShuhui中扩展Map的一个函数很实用，如下：

	fun Map<String, Any>.toBundle(): Bundle {
	    val bundle = Bundle()
	    this.forEach { entry ->
	      when (entry.value) {
	        is Int -> bundle.putInt(entry.key, entry.value as Int)
	        is String -> bundle.putString(entry.key, entry.value as String)
	        is Long -> bundle.putLong(entry.key, entry.value as Long)
	        is Short -> bundle.putShort(entry.key, entry.value as Short)
	        is Float -> bundle.putFloat(entry.key, entry.value as Float)
	        is Double -> bundle.putDouble(entry.key, entry.value as Double)
	        is Byte -> bundle.putByte(entry.key, entry.value as Byte)
	        is Parcelable -> bundle.putParcelable(entry.key, entry.value as Parcelable)
	        is Serializable -> bundle.putSerializable(entry.key, entry.value as Serializable)
	      }
	    }
    	return bundle
  	}

这样我们可以将Map对象转换成Bundle,如何使用需要脑动大开了

#### 多参数函数

在Java中我们有一种函数是多参数，通过在参数类型后面添加 **...**

	public int sum(int... args) {
		int sum = 0;
		for (int i = 0; i < args.length; i++) {
			sum += args[i]
		}
		return sum;
	}

在Kotlin中使用**vararg**关键字来替代 **...**

    private fun sum(vararg addends: Int): Int {
        var result = 0
        for (addend in addends) {
            result += addend
        }
        return result
    }

## 关于集合

```
// 只读
var map = mapOf("a" to 1, "b" to 2)

// 可以动态修改
var map = mutableMapOf<String, String>().apply {
	put("a", "1")
	put("b", "2")
}

```

## 关于委托属性

[委托属性+Sp使用](https://blog.csdn.net/zhang___yong/article/details/78355519)
[玩转委托属性](https://www.jianshu.com/p/306bdc2bac3f)

	setValue(thisRef: Any?, prop: KProperty<*>, value:  String)

示例

```
class Example {

	val token by Sp()

	class Sp() {
		operator fun getValue(thisRef: Any?, prop: KProperty<*>): String {
	        return "$thisRef, thank you for delegating '${prop.name}' to me!"
	    }

	    operator fun setValue(thisRef: Any?, prop: KProperty<*>, value:  String) {
	        println("$value has been assigned to ${prop.name} in $thisRef")
	    }
	}
}
```
说明下参数，thisRef是持有属性的对象，上面例子就是Example, prop就是变量token， value就是给token赋的值

## 关于@JvmStatic

用于在Java中调用kotlin静态变量和方法的方式与Java一致

[https://blog.csdn.net/maosidiaoxian/article/details/81778057](https://blog.csdn.net/maosidiaoxian/article/details/81778057)

## 关于@JvmOverloads

使用该注解，一个方法或者构造自动所有参数重载
[https://www.jianshu.com/p/72d1959a7c56](https://www.jianshu.com/p/72d1959a7c56)

## 关于Delegates.notNull()

用于对变量如果使用时候会抛出`throw IllegalStateException(`

示例： 

	var context: Context by Delegates.notNull()

如果在使用context时候为null则会抛出异常

## 关于open

使用open修饰的类可以被继承

## 关于内联扩展函数

参考[Kotlin系列之let、with、run、apply、also函数的使用](https://blog.csdn.net/u013064109/article/details/78786646#4)

1. **let**

`let`主要用于对空判断，在函数块内通过it替代对象本身，返回函数块最后一行或者return表达式

```
	activity?.let {
		it.startActivity(...)
	}
```

2. **with**

`with`用于调用同一个类，省略类名重复，可以在代码块中直接调用方法,

```
val responseBody = response.body()
with(responseBody) {
	println(string())
}
// 等同于responseBody.string()
```

3. **run**

具备了let和with优点，可以用于对空的判断，还可以在代码块中直接调用方法，返回值为最后一行或者return

```
val value = activity?.run {
	startActivity(...)
	"ccc"
}
println(value) // ccc
```

4. **apply**

用法与run类似，不同的是返回的是对象本身

```
val value = activity?.run {
	startActivity(...)
}

// value为activity
```

5. **also**

用法与let类似，返回的是对象本身

```
val value = activity?.also {
	startActivity(...)
}
// value为activity
```
## 关于