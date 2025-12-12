---
title: xampp配置多域名
date: 2018-10-10 00:00:00
tags:
  - 笔记
  - 笔记
---

修改hosts文件

新加一条记录：127.0.0.1 php.travel.com
window:
    文件位置：系统->“windows”->“System32”->“drivers”->“etc”
mac:
    文件位置：打开Finder->前往文件夹->输入“/private/etc/”
    终端操作：sudo nano /private/etc/hosts

启动Apache的虚拟主机

下载地址：
https://www.apachefriends.org/download.html

修改httpd.conf

一、搜索vhost
搜索到#LoadModule vhost_alias_module modules/mod_vhost_alias.so，将#删除
搜索到#Include conf/extra/httpd-vhosts.conf，将#删除
二、防止403权限问题，将

```

AllowOverride none
Require all denied

```

改成

```

#AllowOverride none
#Require all denied
Order allow,deny
Allow from all

```

创建项目目录

项目文件夹任意位置
比如：在workgit文件夹里新建了一个php-demo文件夹，并在此新建一个hello.php的文件，
hello里写入<?php echo “phpd123456” ?> 方便测试

修改httpd-vhost配置文件

文件地址 xampp/etc/extra

重启Apache检测是否成功

修改hosts文件

启动Apache的虚拟主机

修改httpd.conf

创建项目目录

修改httpd-vhost配置文件

重启Apache检测是否成功

FEATURED TAGS

笔记

FRIENDS

miumiu