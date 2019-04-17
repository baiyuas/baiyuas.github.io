---
title: 关于Docker那些事
date: 2018-02-09 19:50:38
categories: Docker
tags:
    - Docker
---

## 关于基础命令

1. 查看运行的程序

	$ docker ps -a
	
-a 查看运行和停止状态的容器
	
2. 运行程序

	$ docker run -p port --name container-name -v dir:dir -d image-name

--name 为容器取名称
-v 挂载文件或者文件夹
-d 执行后控制台可以继续输入命令

3. 搜索影像

	$ docker search

4. 下载影像

	$ docker pull name:tag

5. 查看本地影像列表

	$ docker images;
	
6. 删除影像
	
	$ docker rmi image-id
	$ docker rmi $(docker images -q) //删除所有

7. 停止容器

	$ docker stop container-name/container-id

8. 启动容器

	$ docker start container-name/container-id

9. 删除容器

	$ docker rm container-id
	$ docker rm $(docker ps -a -q) // 删除所有

10. 查看容器日志

	$ docker log container-name/container-id

11. 登录容器

	$ docker exec -it container-name/container-id bash
	
[更多](https://blog.csdn.net/a33130317/article/details/80884967)


## 关于Mysql

[更多参考](http://www.fwhyy.com/2018/05/docker-installs-mysql-to-mount-external-data-and-configuration/)
[官方配置](https://hub.docker.com/_/mysql)
1. 安装

	$ docker pull mysql:5.6

2. 运行mysql

	$ docker run -p 3306:3306 --name mysql5.6 -e MYSQL_ROOT_PASSWORD=baiyuas123 -d mysql:5.6
	$ docker run -d -p 4306:3306 
	-e MYSQL_USER="fengwei" 
	-e MYSQL_PASSWORD="pwd123" 
	-e MYSQL_ROOT_PASSWORD="rootpwd123" 
	--name mysql001 mysql/mysql-server
	--character-set-server=utf8 
	--collation-server=utf8_general_ci 
	--default-authentication-plugin=mysql_native_password
	$ docker run -d -p 4306:3306 
	--restart always // 开机启动
	--privileged=true // 启动时候需要的权限
	--name mysql001
	-e MYSQL_USER="fengwei" 
	-e MYSQL_PASSWORD="pwd123" 
	-e MYSQL_ROOT_PASSWORD="rootpwd123"
	-v=/mysqltest/config/my.cnf:/etc/my.cnf 
	-v=/mysqltest/data:/var/lib/mysql 
	mysql/mysql-server

-v 可以指定挂载的目录或者文件
-d 后台运行
-e MYSQL_USER=”fengwei”：添加用户fengwei
-e MYSQL_PASSWORD=”pwd123”：设置fengwei的密码伟pwd123
-e MYSQL_ROOT_PASSWORD=”rootpwd123”：设置root的密码伟rootpwd123
–character-set-server=utf8：设置字符集为utf8
–collation-server=utf8_general_ci：设置字符比较规则为utf8_general_ci

3. 拉取影像

可以先到docker Hub网站搜索，然后通过命令下载
	
	$ docker pull mysql:5.6(影像:Tag)

4. 在容器内部执行命令，修改mysql配置文件


5. 查看日志

	$ docker logs mysql5.6(运行时候配置的名称--name)

## 关于nginx

1. 安装

	$ docker pull nginx // 默认获取最新

2. 运行容器

	sudo docker run --name nginx81  -p 81:80 \
	-v $PWD/nginx/html:/usr/share/nginx/html \
	-v $PWD/nginx/logs:/var/log/nginx \
	-v $PWD/nginx/conf.d:/etc/nginx/conf.d \
	-v $PWD/nginx/config/nginx.conf:/etc/nginx/nginx.conf \
	-d nginx:latest

[更多docker运行nginx参考](https://hub.docker.com/_/nginx)
[Nginx中文文档](http://www.nginx.cn/doc/)
[Nginx官方文档](http://nginx.org/en/docs/beginners_guide.html)

## 常见问题

1. 登录失败Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.

转自[https://www.cnblogs.com/informatics/p/8276172.html](https://www.cnblogs.com/informatics/p/8276172.html)

使用sudo获取管理员权限，运行docker命令

2. Ubuntu 16.04系统上修改Docker镜像的存储路径

[方案一](https://blog.csdn.net/reinnovation/article/details/80058233)

[方案二](https://blog.csdn.net/u010053962/article/details/81875692)

3. 执行命令Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

没有启动docker服务 $ sudo systemctl start docker

4. docker: Error response from daemon: error creating overlay mount to /media/leo/Software/Ubuntu/Application/lib/docker/docker/overlay2/b1300d3030c1552f6bfc417e42461a336237f6dd9d12ad6a2f34bcfa51941dcf-init/merged: invalid argument.

这个问题是磁盘驱动有问题，修改docker的磁盘启动，
[几种磁盘驱动结束](https://blog.csdn.net/vchy_zhao/article/details/70238690)
我的默认是overlay2 后修改为aufs就可以启动了
修改方式
	
	$ sudo vim /etc/systemd/system/docker.service.d/docker-overlay.conf // 如果没有则创建
配置如下信息

```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd --graph=/media/leo/Software/Ubuntu/Application/lib/docker/docker/ --storage-driver=aufs
```

网上大多数都是centerOs修改/etc/sysconfig/docker-storage 这个在ubuntu中对应的是/etc/defautl/docker

5. 





