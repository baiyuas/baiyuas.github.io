---
title: Ubuntun使用之随笔记
date: 2018-02-09 19:50:38
categories: Linux
tags:
	- 笔记
    - Ubuntu
---


笔记记录基于Ubuntu Desktop 18.04版本

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

<!-- more -->

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

* teamviewer

先去官网下载[点我去下载](https://www.teamviewer.com/zhcn/download/linux/)，下载后通过如下命令安装

	$ sudo dpkg -i teamviewer_13.2.13582_amd64.deb

首次安装时候需要一些qt依赖，所以会安装失败，通过如下命令安装依赖

	$ sudo apt install -f
	$ sudo dpkg -i teamviewer_13.2.13582_amd64.deb

这样就可以完成安装，有时候安装后无法启动，一般是权限问题，打开终端通过命令`teamviewer`启动，根据提示给对应文件夹权限就可以了

* python-pip

这是一个python依赖包管理工具，

	$ apt install python-pip

* tensorflow

通过安装的`python-pip`安装tensorflow

	$ pip install tentsorflow

* Flutter

这个是开发移动的SDK工具，这个需要使用影像下载，不然国内无法下载

```
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
git clone -b dev https://github.com/flutter/flutter.git
export PATH="$PWD/flutter/bin:$PATH"
cd ./flutter
flutter doctor

```
在执行`flutter doctor`时候，如果没有安装`curl`会报错，需要先安装`curl`

	$ sudo apt install curl

另外这里配置`flutter`的环境变量是临时的，需要在配置文件将`flutter`的变量配置进去

	$ vim ~/.bashrc

>	export PUB_HOSTED_URL=https://pub.flutter-io.cn
	export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
	export PATH=/home/baiyu/Android/flutter/bin:$PATH

* net-tools

我们经常会使用查看ip，Ubuntu18默认没有ifconfig 命令，需要安装net-tools

	$ sudo apt install net-tools

* Apache2

Apache2 Web服务器，需要搭建自己服务器的可以安装

	$ sudo apt install apache2

关于Apache2的几个命令

	$ service apache2 status // 查看Apache2运行状态
	$ apache2 -v // 查看apache2版本
	$ service apache2 start[restart|stop] // 启动、关闭、重启服务

配置文件所在目录 `/etc/apache2`

整合tomcate可以参考[apache2整合tomcat进行项目部署](https://blog.csdn.net/qq_37317845/article/details/80899548)


* 花生壳

下载地址[花生壳客户端](https://hsk.oray.com/download/#linux)

安装后phddns相关命令

	$ phddns status // 查看状态
	$ phddns restart // 重启
	$ phddns start // 启动
	$ phddns stop // 停止

* openssh-server

ssh服务器，安装命令
	
	$ sudo apt install openssh-server


* virtualbox

[官网下载](https://www.virtualbox.org/wiki/Downloads)下载昊安装包后，通过`dpkg -i xxx.deb`安装。
安装时候提示缺少依赖，按着提示安装即可

我安装的是Win7系统，在新建虚拟机后，启动总是提示`No bootable medium found! System halted`错误，网上多数都是因为没有配置ios影像，我也配置了，后来发现需要安装***安装版***的系统影像，分享个链接[http://down.u567.cn:8080/soft/Windows%207%20x64.iso](http://down.u567.cn:8080/soft/Windows%207%20x64.iso)
下载这个启动就可以了

* lrzsz

Linux上可以代替ftp上传下载的命令	

	$ rz
	$ sz


* vim 插件Vundle

安装地址：[https://github.com/VundleVim/Vundle.Vim#quick-start(https://github.com/VundleVim/Vundle.Vim#quick-start)

命令无效解决:[https://blog.csdn.net/silinga/article/details/80059006](https://blog.csdn.net/silinga/article/details/80059006)

插件库：[http://vim-scripts.org/vim/scripts.html](http://vim-scripts.org/vim/scripts.html)

[Mardkown语法高亮](https://github.com/plasticboy/vim-markdown)
[Markdown实时预览](https://github.com/suan/vim-instant-markdown)
[代码智能补全](https://github.com/Valloric/YouCompleteMe#linux-64-bit)
[代码智能补全2](https://www.cnblogs.com/Suzzz/p/4071880.html)

* redshift 护眼软件

	$ sudo add-apt-repository ppa:dobey/redshift-daily
	$ sudo apt-get install redshift-gtk

* fish-shell

这是一个很好用的bash终端，比Ubuntu默认的好用，带有记忆功能
更多使用参考[如何在 Linux 中安装、配置和使用 Fish Shell](https://mp.weixin.qq.com/s?__biz=MzAxODI5ODMwOA==&mid=2666543850&idx=1&sn=fa2e99241d893294d303b4e24854b1c5&chksm=80dcfe41b7ab7757792a1f5710a0f83aa8f25a1bb948f372d9821ac1f652929c077d1926fc77&mpshare=1&scene=1&srcid=0321uBSy2q7DGTSWBypTvA6e&pass_ticket=TibiJijFE8acm%2BGL96Px4ivFOw17o4vfUD3hdvDYJRAisKw6r0BE%2B24gmCs91597#rd)

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

### 启动sshd服务失败

电脑断电后，重启，IP 被占用，然后修改IP,没有修改sshd_config中的ip所以失败了


### 创建自启动服务

网上找了很多，比较详细的是这个[ubuntu-server-18.04 设置开机启动脚本](http://www.cnblogs.com/defifind/p/9285456.html)

### 解决vbox模拟器和android模拟器不能共存、

命令
	$ sudo rmmod kvm_intel kvm

### 关闭Apache开机自启动

[https://blog.csdn.net/geeksoarsky/article/details/80083557](https://blog.csdn.net/geeksoarsky/article/details/80083557)

### 管理登录页面用户

在启动电脑进入登录页面后你添加的用户都会显示在列表中，如果只想展示一个用户，需要进入`/var/lib/AccountsService/users`目录，该目录下列出了所有用户，打开对应的用户文件

> [User]
XSession=
SystemAccount=false

只要将SystemAccount = true即可


### Firefox无法在先播放音乐的问题

	sudo apt-get install ubuntu-restricted-extras


### 重启sshd服务失败

在执行`systemctl restart sshd.service`时候报错***Failed to restart sshd.service: Interactive authentication required.***

解决：权限不足

	$ sudo systemctl restart sshd.service


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

### ssh 登录sign_and_send_pubkey: signing failed: agent refused operation leo@liupengcheng.iok.la: Permission denied (publickey).

```
[root@node224 .ssh]# ssh node228
sign_and_send_pubkey: signing failed: agent refused operation
root@node228's password: 
 
[root@node224 .ssh]# eval "$(ssh-agent -s)"
Agent pid 5208
[root@node224 .ssh]# ssh-add
Identity added: /root/.ssh/id_rsa (/root/.ssh/id_rsa)
[root@node224 .ssh]# ssh node228
Last login: Fri May 11 16:25:20 2018 from 某个ip
--------------------- 
作者：tiankong_12345 
来源：CSDN 
原文：https://blog.csdn.net/tiankong_12345/article/details/80283404 
版权声明：本文为博主原创文章，转载请附上博文链接！
```


## 常用操作

### 设置`shell`别名

在使用Ubuntu时候有些命令频繁使用，比如

```
$ cd xxx/xxx
$ ./gradlew assembleRelease
```
上面是我编译apk时候命令，打开终端总要切换到工作目录在执行编译，使用别名就简单多了
在家目录下.bashrc文件，编辑该文件，添加
	
	alias buildApk='cd xxx/xxx&&./gradlew assembleRelease'

`&&`代表上一个命令执行成功后在执行后面命令，`||`表示上一个命令执行失败后执行下一个命令
`;`不管执行结果

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

命令

	$ usermod -a -G gourp user

`-a`将用户添加到组，而不用离开其他组

* 切换到上次的目录

命令

	cd -

* 查看当前目录容量大小

命令
	
	$ sudo du -sh
	20G

* 查看磁盘容量

命令
	
	$ df -hl

* 搜索文件

命令
	
	$ find .  xx.txt // 查找但前目录以及子目录中xx.txt文件，可以使用通配符
	$ find /home/leo/  xx.txt //查找指定目录 /home/leo 下的xx.txt文件

* 查看版本号

命令

	$ cat /etc/issue // Ubuntu
	$ cat /proc/version 
	$ uname -a

* systemctl

系统服务管理命令

查看服务状态

	$ systemctl status xx.service

启动，停止，重启服务

	$ systemctl start[restart|stop] xx.service

查看ssh服务状态

	$ ps -e | grep ssh

* opendir (自定义命令)

该命令是在终端打开当前目录的文件夹。需要安装nautilus-open-terminal，默认Ubuntu自带
在/usr/bin下增加opendir

	$ sudo vim /usr/bin/opendir

编辑如下内容
```
if [ -n "$1" ]; then
	nautilus "$1" > /dev/null 2>&1
else
	nautilus "$(pwd)" > /dev/null 2>&1
fi
```

	$ sudo chmod 711 /usr/bin/opendir
	$ sudo chown xxx（当前用户） /usr/bin/opendir

之后在终端执行opendir就可以在文件管理器打开当前目录了
	
* ssh-copy-id

ssh免密码登录，想要通过ssh免密码远程登录另外一个主机，需要将自己公钥追加到远程主机中就需要这个命令，开始时候使用这个命令配置后，登录还是需要密码。网上找了好久，多数都是修改权限的，默认一般权限是没有问题，最后找到解决问题的答案需要绕过SSH公钥检查

	$ ssh-copy-id -o StrictHostKeyChecking=no xxx@xxx.xxx.x.xx

这样就可以进行免密码登录了


* [内存和Cpu相关命令](https://www.cnblogs.com/xd502djj/archive/2011/03/01/1968041.html)

查看内存使用状况，类似Windows中任务管理器中效果

	$ top 

查看内存整体使用情况

	$ free [-m ][- g] // 或者cat /proc/meminfo

查看端口使用情况

	$ netstat -tunlp |grep 80


## 防火墙

1. 安装

sudo apt-get install ufw

2. 启用

sudo ufw enable

sudo ufw default deny

运行以上两条命令后，开启了防火墙，并在系统启动时自动开启。关闭所有外部对本机的访问，但本机访问外部正常。

3. 开启/禁用

sudo ufw allow|deny [service]

打开或关闭某个端口，例如：

	$ sudo ufw allow smtp　允许所有的外部IP访问本机的25/tcp (smtp)端口
	$ sudo ufw allow 22/tcp 允许所有的外部IP访问本机的22/tcp (ssh)端口
	$ sudo ufw allow 53 允许外部访问53端口(tcp/udp)
	$ sudo ufw allow from 192.168.1.100 允许此IP访问所有的本机端口
	$ sudo ufw allow proto udp 192.168.0.1 port 53 to 192.168.0.2 port 53
	$ sudo ufw deny smtp 禁止外部访问smtp服务
	$ sudo ufw delete allow smtp 删除上面建立的某条规则

4.查看防火墙状态

	$ sudo ufw status

一般用户，只需如下设置：

	$ sudo apt-get install ufw
	$ sudo ufw enable
	$ sudo ufw default deny

以上三条命令已经足够安全了，如果你需要开放某些服务，再使用sudo ufw allow开启。

开启/关闭防火墙 (默认设置是’disable’)

	$ sudo ufw enable|disable

转换日志状态

	$ sudo ufw logging on|off

设置默认策略 (比如 “mostly open” vs “mostly closed”)

	$ sudo ufw default allow|deny

许 可或者屏蔽端口 (可以在“status” 中查看到服务列表)。可以用“协议：端口”的方式指定一个存在于/etc/services中的服务名称，也可以通过包的meta-data。 ‘allow’ 参数将把条目加入 /etc/ufw/maps ，而 ‘deny’ 则相反。基本语法如下：

	$ sudo ufw allow|deny [service]

显示防火墙和端口的侦听状态，参见 /var/lib/ufw/maps。括号中的数字将不会被显示出来。

	$ sudo ufw status

UFW 使用范例：

允许 53 端口

	$ sudo ufw allow 53

禁用 53 端口

	$ sudo ufw delete allow 53

允许 80 端口

	$ sudo ufw allow 80/tcp

禁用 80 端口

	$ sudo ufw delete allow 80/tcp

允许 smtp 端口

	$ sudo ufw allow smtp

删除 smtp 端口的许可

	$ sudo ufw delete allow smtp

允许某特定 IP

	$ sudo ufw allow from 192.168.254.254

删除上面的规则

	$ sudo ufw delete allow from 192.168.254.254

以上防火墙内容转自[https://www.cnblogs.com/OnlyDreams/p/7210914.html](https://www.cnblogs.com/OnlyDreams/p/7210914.html)