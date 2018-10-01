---
title: 基于Window安装Weblogic
date: 2018-03-07 16:46:38
categories: Web
tags:
    - Weblogic
---

1，	Weblogic版本：
•weblogic-wls1035_oepe111172_win32
2.	安装环境
Window 10_64位，jdk-7u51-windows-i586
3，	安装步骤
1)	点击安装文件，进入安装界面
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-1.png)
2)	下一步
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-2.png)
3)	选择创建新的中间件目录(如果以前安装过删除了但是还有目录会默认选择使用现有的中间件目录)，下一步
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-3.png)
4)	去掉安装更新的勾选，下一步：
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-4.png)
5)	默认典型安装，下一步
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-5.png)
6)	下一步：
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-6.png)
7)	下一步:
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-7.png)
8)	下一步:
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-8.png)
9)	开始安装:
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-9.png)
10)	安装成功后，运行Quickstart，点击完成，进入服务器配置
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-10.png)

11)	启动配置界面后，选择第一个Getting Started with Weblogic ……
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-11.png)
12)	创建一个新的域，点击下一步
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-12.png)
13)	默认下一步:
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-13.png)

14)	指定域名和域位置(这里配置的域名和域位置要和MyEclipse中配置的一致，在配置MyEclipse中还会说明，这里记住位置和域名)，可以使用默认的，下一步:
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-14.png)
15)	输入用户名和密码(这里用户名和密码用于控制台登录，需记住)，点击下一步:
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-15.png)
16)	默认下一步: 
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-16.png)
17)	勾选管理服务器，受管服务器。下一步
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-17.png)
18)	下一步:
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-18.png)

19)	下一步:
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-19.png)
20)	下一步:
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-20.png)
21)	下一步:
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-21.png)
22)	创建:
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-22.png)


23)	勾选启动管理服务器，点击完成
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-23.png)
	•等服务启动
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-25.png)



4，	数据源配置
1)	打开浏览器，进入Weblogic控制台界面，http://localhost:7001/console
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-26.png)
2)	输入用户名密码(在创建Weblogic服务器时候添加的可参考输入用户名和密码(这里用户名和密码用于控制台登录，需记住)，点击下一步:)，登录
 ![](http://ovpqrf5pq.bkt.clouddn.com/weblogic-27.png)