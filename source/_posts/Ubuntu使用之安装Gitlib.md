---
title: Ubuntu使用之安装Gitlib
date: 2018-03-07 16:46:38
categories: Linux
tags:
    - Ubuntu
    - Gitlib
---

安装基于Ubuntu Server 64 16.04版本

## 安装配置依赖项
> sudo apt-get install curl openssh-server ca-certificates postfix

## 添加GitLab仓库,并安装到服务器上
> curl -sS http://packages.gitlab.cc/install/gitlab-ce/script.deb.sh | sudo bash

在安装gitlab-ce时候使用清华大学服务影像下载快
https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/

> curl https://packages.gitlab.com/gpg.key 2> /dev/null | sudo apt-key add - &>/dev/null

将deb https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/ubuntu xenial main写入到/etc/apt/sources.list.d/gitlab_gitlab-ce.list注释掉之前的
> sudo apt-get update
sudo apt-get install gitlab-ce

## 启动GitLab
> sudo gitlab-ctl reconfigure

启动后，登录http://ip/users/sign_in
初次会提示设置root管理员的密码
