---
title: Ubuntu使用之安装Subversion
date: 2018-03-07 16:46:38
categories: Linux
tags:
    - Ubuntu
    - Gitlib
---

安装基于Ubuntu Server 64 16.04版本

<!-- more -->

# 安装

输入下面命令安装
>  $ sudo apt-get install subversion

# 创建仓库

>  $ cd /home/baiyu
>  $ sudo mkdir SVNRep
>  $ sudo svnadmin create

**svnadmin: E205000: 需要版本库参数**
>  $ sudo svnadmin create repository

# 配置

> $ cd repository/config
> $ vim svnserve.conf
\# password-db = passwd
\# authz-db = authz
\# anon-access = read
\# auth-access = write
去掉上面#将内容顶格

#### 配置用户

>  $ vim passwd
[users]
baiyu = heihei

#### 配置规则

>  $ vim authz
[groups]
android = liupengcheng
[/]
liupengcheng = rw //单个用户的权限配置
@android = rw //组的权限配置

# 启动和停止

#### 启动

>  $ sudo svnserve -d -r /mnt/hgfs/F/SVNRep/repository/
-d 守护进程
-r 指定url访问的根路径

#### 查看SVN是否运行

>  $ sudo ps -ef|grep svn

#### 停止

>  $ sudo killall svnserve

# 访问
我这里使用的是小乌龟
地址输入*svn://ipaddress*


