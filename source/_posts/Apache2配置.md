title: Apache2配置
date: 2018-02-09 19:50:38
categories: Linux
tags:
    - 笔记
    - Linux
---

Apache2在ubutnu18.0.4中默认安装，配置文件路径
	
	/etc/apache2/

看下所有配置信息

```
drwxr-xr-x   8 root root  4096 4月   9 17:31 ./
drwxr-xr-x 133 root root 12288 4月   9 10:30 ../
-rw-r--r--   1 root root  7236 4月   9 17:15 apache2.conf
drwxr-xr-x   2 root root  4096 4月   9 17:27 conf-available/
drwxr-xr-x   2 root root  4096 10月 16 23:08 conf-enabled/
-rw-r--r--   1 root root  1782 6月  28  2018 envvars
-rw-r--r--   1 root root 31063 6月  28  2018 magic
drwxr-xr-x   2 root root 12288 4月   9 17:41 mods-available/
drwxr-xr-x   2 root root  4096 4月   9 17:44 mods-enabled/
-rw-r--r--   1 root root   320 4月   9 17:20 ports.conf
drwxr-xr-x   2 root root  4096 4月   9 17:30 sites-available/
drwxr-xr-x   2 root root  4096 10月 17 00:29 sites-enabled/

```

apache2.conf配置主要文件

conf-available/ 可用配置文件
conf-enabled/ 起作用的配置文件，其实就是conf-available/ 中可以使用配置文件的软链接
mods-available/ 可用的库
mods-enabled/ 起作用的库
sites-available/ 可配置站定
sites-enabled/  起作用的站点

sudo a2enmod ssl

这条命令相当于
sudo ln -s /etc/apache2/mods-available/ssl.load /etc/apache2/mods-enabled
sudo ln -s /etc/apache2/mods-available/ssl.conf /etc/apache2/mods-enabled

如果没有a2enmod指令

开启SSH

	$ sudo a2enmod ssl
	$ sudo a2ensite default-ssl
	$ systemctl reload apache2

如果需要自定义签名证书，需要修改配置文件

	/etc/apache2/sites-enabled/default-ssl.conf 

下面的内容

	SSLCertificateFile	/etc/ssl/certs/ssl-cert-snakeoil.pem
	SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key
