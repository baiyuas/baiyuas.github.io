---
title: 深入理解JVM笔记 认识JDK
date: 2017-09-12 11:32:47
categories: "Java进阶"
tags:
	- JDK
---

学习Java这么久，从来没有系统的研究过，都是用什么搞什么。趁着现在时间整理下。

## Java特性
1. 体积小。
> 主要体现在生成的可执行代码小
2. 与平台无关
> 这个主要体现在JVM上。JVM具有可移植性，同时JVM又为Java可执行文件提供的运行环境，所以Java也就成为了可以在不同平台上运行的特点
3. 更加简单的多线程支持
>  Java屏蔽的很多复杂的概念编码，包括临界区和信号量等，这就使得Java线程编程变的更加简单
4. 动态特性
>  

    PS：这个Java特性一般面试时候会问到，很多大公司除了问一些纯技术知识外还会问一些理解类的问题，这个也算其中之一吧

<!-- more -->

## Java不同版本的特性比较
| JDK1.5 | JDK1.6 | JDK1.7 | JDK1.8 |
|:-----:|:-----:|:-----:|:-----:|
| 自动装箱与拆箱 | Web服务元数据 | switch中可以使用字串了 | 允许在接口中有默认方法实现 | 
| 枚举 | 脚本语言支持 | 即泛型实例化类型自动推断 | Lambda表达式 | 
| 静态导入 | JTable的排序和过滤 | 新增一些取环境信息的工具方法 | 函数式接口 |
| 可变参数 | 更简单,更强大的JAX-WS | Boolean类型反转，空指针安全,参与位运算 | 内置函数式接口 |
| 内省 | 轻量级Http Server | 两个char间的equals | Streams |
| for循环 | 嵌入式数据库 Derby | map集合支持E:\DevelopTool\gradle\gradle-4.1-all\gradle-4.1\bin并发请求，且可以写成 Map map = {name:"xxx",age:18}; | Parallel Streams |
| 泛型 |  |  | 时间日期API |
|  |  |  | Map |
|  |  |  | Annotations |

除了上面的还有一些补充知识：
在JDK1.1中使用JavaBean、RMI、数字签名、
在JDK1.2中引入了Swing、2D API
在JDK1.3中增加了java.math包
在JDK1.4中增加了java.util.regex正则包

由此我们可以看出在JDK1.5之后JDK才相对完善了些。我们现在使用的JDK一般都是1.6之上
Java技术体系分为四个部分：
    JavaCard:支持一些Java小程序(Applet)
    JavaME:支持Java程序运行在移动平台
    JavaSE:支持面向桌面应用的开发
    JavaEE:支持面向企业应用，如ERP、CRM
 而我们常用的主要是JavaSE和JavaEE


