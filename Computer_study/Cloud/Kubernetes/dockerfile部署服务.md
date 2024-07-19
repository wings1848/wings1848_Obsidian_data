---
title: dockerfile部署服务
author: wings1848
date: 2024-05-30 16:55:34
categories: 'ChinaSkills云计算笔记'
tags: 
  - 'docker'
keywords:
---
准备
```
# local.repo yum源文件

[yum]
name=yum
baseurl=file:///opt/yum/ 
gpgcheck=0
enabled=1
```

```shell
docker load -i centos_7.9.2009.tar
# 将压缩文件中的centos7.9.2009.tar导入docker本地镜像仓库
```

# 部署MariaDB

+ 编写Dockerfile构建镜像`<tag-name:version>`，要求使用centos7.9.2009镜像作为基础镜像，完成MariaDB数据库的安装，设置UTF-8编码格式,设置root用户的密码为`<password>`，并设置MariaDB数据库开机自启。

```shell 
# init_sql.sh 初始化脚本

#!/bin/bash
mysqld_safe & # 启动mysqld
sleep 5

mysqladmin -u root password '<password>' # 设置root用户密码

mysql -uroot -p<password>

mysql -uroot -p<password> -e "grant all on *.* to 'root'@'%' identified by '<password>';" # 加权限
# flush privileges;

mysql -uroot -p<password> -e "create database <database name>;use <database name>;source <sql file>;" # 导入数据库文件
```

```dockerfile 
# mariadb_dockerfile

from centos:centos7.9.2009
env LC_ALL=en_US.UTF-8 # 设置UTF-8编码的语言环境

run rm -rf /etc/yum.repos.d/*
copy yum /opt/yum
copy local.repo /etc/yum.repos.d/

run yum install mariadb-server -y

copy init_sql.sh <sql file> /opt/
run sh /opt/init_sql.sh


expose 3306
cmd ["mysqld_safe"] # 在容器中以主进程启动mariadb服务
```

```shell
docker build -t <tag-name:version> -f mariadb_dockerfile .
```

---

# 部署Redis

+ 编写Dockerfile构建镜像explorer-redis:v1.0，要求使用centos7.9.2009镜像作为基础镜像，完成Redis服务的安装，修改其配置文件关闭保护模式，并设置Redis服务开机自启。

```dockerfile 
# redis_dockerfile

from centos:centos7.9.2009

run rm -rf /etc/yum.repos.d/*
copy yum /opt/yum
copy local.repo /etc/yum.repos.d/

run yum install redis -y

run sed -i 's/bind 127.0.0.1/bind 0.0.0.0/g' /etc/redis.conf
run sed -i 's/protected-mode yes/protected-mode no/g' /etc/redis.conf

# 或master节点安装redis,修改后copy配置文件
# copy redis.conf /etc/redis.conf

expose 6379
cmd ["redis-server","/etc/redis.conf"]
```

```shell
docker build -t <tag-name:version> -f redis_dockerfile .
```

# 部署Nginx

+ 编写Dockerfile文件构建镜像explorer-nginx:v1.0，要求基于centos7.9.2009镜像,完成Nginx服务的安装和配置，并设置Nginx服务开机自启。

```dockerfile
# nginx_dockerfile

from centos:centos7.9.2009

run rm -rf /etc/yum.repos.d/*
copy yum /opt/yum
copy local.repo /etc/yum.repos.d/

run yum install nginx -y

add nginx/app.tar.gz /
copy nginx/nginx.conf /etc/nginx/

expose 80
cmd ["nginx","-g","daemon off;"]
```

```shell
docker build -t <tag-name:version> -f nginx_dockerfile .
```

# 部署ERP

+ 编写 Dockerfile 构建镜像erp-server:v1.0，要求使用centos7.9.2009 镜像作为基础镜像，完成 JDK 环境的安装，启动提供的 jar 包，并设置服务开机自启。

```dockerfile
# erp_java_dockerfile

from centos:centos7.9.2009

run rm -rf /etc/yum.repos.d/*
copy yum /opt/yum
copy local.repo /etc/yum.repos.d/

run yum install java-* -y

copy app.jar /opt/

expose 9999
cmd ["java","-jar","/opt/app.jar"]
```

```shell
docker build -t <tag-name:version> -f erp_dockerfile .
```

# 部署Explorer(kodexplorer)

+ 编写Dockerfile构建镜像explorer-server:v1.0，要求使用centos7.9.2009镜像作为基础镜像，完成HTTP和PHP服务的安装，然后将kodexplorer4.37.zip解压到/var/www/html目录下，修改HTTP服务的监听地址为本机，并设置HTTP服务开机自启。

***可获分***

```dockerfile
# kod_dockerfile

FROM centos:centos7.9.2009

RUN rm -rf /etc/yum.repos.d/*
COPY local.repo /etc/yum.repos.d/

COPY yum /opt/yum
RUN yum install httpd -y

RUN sed -i 's/#ServerName.*/ServerName localhost:80/g' /etc/httpd/conf/httpd.conf

CMD ["httpd","-D","FOREGROUND"]
```

```shell
docker build -t <tag-name:version> -f kod_dockerfile .
```