---
title: Kotlin 笔记
date: 2018-03-07 16:46:38
categories: Android
tags:
	- Kotlin
---

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

