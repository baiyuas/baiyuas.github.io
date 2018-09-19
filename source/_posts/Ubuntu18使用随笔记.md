---
title: Ubuntun18-随笔记
date: 2018-02-09 19:50:38
categories: Linux
tags:
	- 笔记
    - Ubuntu18
---

## 软件安装

我使用的Ubuntu 安装这些常用软件

* yddict 词典

安装命令

	npm install -g yddict

安装后可使用命令
	
```
	yd hello

	英 [həˈləʊ] 美 [helˈō] 
	int. 喂；哈罗
	n. 表示问候， 惊奇或唤起注意时的用语
	n. (Hello)人名；(法)埃洛
	Hello, who's speaking, please?
	喂， 请问你是谁呀?
	The American walked to a telephone booth,"Hello. Is that the bank?
	那个美国人走到公用电话间旁打电话:"喂,银行吗?
	She never passes without stopping to say hello.
	她从这儿经过时没有一次不停下来问候一番。
```

* npm

安装命令

	$ sudo apt install npm

* python

安装命令

	$ sudo apt install python

* git

安装命令

	$ sudo apt install git

* sqlite3

安装命令

	$ sudo apt install sqlite3

* Jdk

自带的JDK 是openJdk 10在开发Android时候使用gradlew命令会和项目使用的1.8冲突，编译失败

官网下载[https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](https://www.oracle.com/technetwork/java/javase/
downloads/jdk8-downloads-2133151.html)

解压到指定目录

修改默认Java

	$ update-alternatives --install /usr/bin/java java $(your_dir)/jdk1.6.0_12/bin/java 300
	$ update-alternatives --install /usr/bin/javac javac $(your_dir)/jdk1.6.0_12/bin/javac 300

配置默认Jdk

	$ update-alternatives --config java

* vim

安装命令

	$ sudo apt install vim

解决乱码
[vim乱码](https://blog.csdn.net/zhangjiarui130/article/details/69226109)


* myusql-workbench

需要官网下载deb包，或者自己添加软件源，通过`apt`安装

	$ sudo dpkg -i mysql-workbench-community_8.0.12-1ubuntu18.04_amd64.deb 
	$ sudo apt -f install
	$ sudo dpkg -i mysql-workbench-community_8.0.12-1ubuntu18.04_amd64.deb 
	彻底卸载
	$ dpkg -P mysql-workbench


## apt 软件安装命令

添加软件源

	$ sudo add-apt-repository -r ppa:fcitx-team/nightly

删除软件源

	$ sudo add-apt-repository -r ppa:fcitx-team/nightly


## dpkg 软件安装命令

dpkg命令
查看列表

	dpkg -l

安装

	dpkg -i 

卸载

	dpk -P


## 常见问题

### 使用git文件名问题

使用`git status`命令后文件名显示`Ubuntu\344\275\277\347\224\250\347\254\224\350\256\260.md`，使用如下命令解决

	$ git config --global core.quotepath false

### 系统卡顿

主要原因是swappniness参数影响，   在UBUNTU系统里面，并不是你的物理内存全部耗尽之后，系统才使用swap分区！系统的swappiness设定值，对如何使用swap分区是有着很大的联系。 

swappiness=0 的时候表示最大限度使用物理内存，然后才是 swap空间
swappiness＝100的时候表示积极的使用swap分区，并且把内存上的数据及时的搬运到swap空间里面

解决方法：
1. 点击 应用程序 - 附件 - 终端
2. 输入下面内容查看 swappiness 的当前参数（默认应该看到的是60）

命令

    cat /proc/sys/vm/swappiness

3. 输入下面内容，修改这个参数为20（执行这一步只是临时修改，如果重启又会恢复60）

命令

	sudo sysctl vm.swappiness=20

4. 下面的语句可以让你保存你设定的值，首先输入：

命令

	sudo gedit /etc/sysctl.conf

复制代码
然后在底部加入下面内容：

    vm.swappiness=20

### 启动模拟器提示kvm无权限

因为默认用户是非`root`用户，而`/dev/kvm`只有root用户有读写执行权限
将当前用户添加到root组里

	$ sudo usermod -G root currentUser

修改`/dev/kvm`权限，同组用户都可以有读写权限

	$ sudo chomd 771 /dev/kvm

## 常用操作

### 给应用创建桌面快捷方式

所有应用的快捷方式都存储在 `/usr/share/applications/` 目录下，参考其中一个创建就可以
这里以我创建的android-studio的桌面快捷方式为例
在`/usr/share/applications/`创建文件android-studio,内容为

```
	[Desktop Entry]
	Type=Application  // 类型为应用
	Name=android-studio // 显示名菜
	Exec=android_studio //执行程序
	Icon=studio  //图标
```

这里Exec 配置android-studio，我已经创建了软链接 

	$ sudo ln -s xxx/studio.sh /usr/bin/android_studio

### 快捷键使用

* 显示活动大纲

>	Alt+F1

* 启动终端

>	Ctrl+Alt+T

* 切换桌面
	
>	Win+PgUp(PgDn, Home, End)

* 菜单列表

>	Win+A

* 打开通知列表

>	Win+V

* 截屏快捷键

```
	Shift+Print // 选取截屏到图片目录
	Ctrl+Print // 复制截屏到剪切板
	Print // 屏幕截屏到图片目录
	Ctrl+Print // 复制屏幕截屏到剪切板
```

* 录屏

>	Ctrl+Shift+Alt+R

* 切换窗口位置， 左;右;全;还原

>	Win+Up;Down;Left:Right

### 常用文件目录

* 存储程序的还做面快捷方式

>	/usr/share/applictions

* 设备
	
>	/dev/
	
* 家目录

>	/home/leo/
	/root/

* 命令存储位置

>	/usr/bin

* 外接设备

>	/media

* 配置文件
	
>	/etc/

* 程序库
	
>	/usr/lib

* 第三方程序

>	/opt/ 我一般在家目录创建一个appliction目录存储

* 所有用户密码

>	/etc/shadow // 文件

### 用到的命令

* 将用户添加到用户组

	usermod -a -G gourp user

`-a`将用户添加到组，而不用离开其他组

* 切换到上次的目录

	cd -
	