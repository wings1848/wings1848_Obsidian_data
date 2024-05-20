```
# /root/local.repo yum源文件

[yum]
name=yum
baseurl=file:///opt/yum/ 
gpgcheck=0
enabled=1
```

```txt
使用的yum包可能在/FileServer/compose/Hyperf.tar.gz中
或/docker-package/yum.tar.gz中

把yum包存放在/root/yum

docker镜像用Hyperf.tar.gz中的centos_7.9.2009.tar包
```

```shell
docker load -i centos_7.9.2009.tar # 导入centos7.9.2009镜像
```

# 部署MariaDB

+ 编写Dockerfile构建镜像explorer-mysql:v1.0，要求使用centos7.9.2009镜像作为基础镜像，完成MariaDB数据库的安装，设置root用户的密码为root，并设置MariaDB数据库开机自启。

```shell 
# init_sql.sh 初始化脚本

#!/bin/bash
mysqld_safe & # 启动mysqld
sleep 5

mysqladmin -u root password '<password>' # 设置root用户密码

mysql -uroot -p<password>

mysql -uroot -p<password> -e "grant all on *.* to root@'%' identified by '<password>';flush privileges;" # 加权限

mysql -uroot -p<password> -e "create database <database name>;use <database name>;source <sql file>;" # 导入数据库文件
```

```dockerfile 
# mariadb_dockerfile

FROM centos:7.9.2009

RUN rm -rf /etc/yum.repos.d/*
COPY yum /opt/
COPY local.repo /etc/yum.repos.d/
RUN yum install mariadb-server -y

COPY init_mariadb.sh <sql file> /opt/
RUN sh /opt/init_mariadb.sh

EXPOSE 3306
CMD ["mysqld_safe"]
```

```shell
docker build -t explorer-mysql:v1.0 -f mariadb_dockerfile /root/
```

---

# 部署Redis

+ 编写Dockerfile构建镜像explorer-redis:v1.0，要求使用centos7.9.2009镜像作为基础镜像，完成Redis服务的安装，修改其配置文件关闭保护模式，并设置Redis服务开机自启。

```dockerfile 
# redis_dockerfile
FROM centos:centos7.9.2009

RUN rm -rf /etc/yum.repos.d/*
COPY local.repo /etc/yum.repos.d/

COPY yum /opt/yum
RUN yum install redis -y

RUN sed -i 's/bind 127.0.0.1/bind 0.0.0.0/g' /etc/redis.conf
RUN sed -i 's/protected-mode yes/protected-mode no/g' /etc/redis.conf

EXPOSE 6379
CMD ["redis-server"]
```

```shell
docker build -t explorer-redis:v1.0 -f redis_dockerfile /root/
```

# 部署Nginx

+ 编写Dockerfile文件构建镜像explorer-nginx:v1.0，要求基于centos7.9.2009镜像,完成Nginx服务的安装和配置，并设置Nginx服务开机自启。

```dockerfile
# nginx_dockerfile

FROM centos:centos7.9.2009

RUN rm -rf /etc/yum.repos.d/*
COPY local.repo /etc/yum.repos.d/

COPY yum /opt/yum
RUN yum install nginx -y

EXPOSE 80
CMD ["nginx","-g","daemon off;"]
```

```shell
docker build -t explorer-nginx:v1.0 -f nginx/nginx_dockerfile .
```

# 部署ERP

+ 编写 Dockerfile 构建镜像erp-server:v1.0，要求使用centos7.9.2009 镜像作为基础镜像，完成 JDK 环境的安装，启动提供的 jar 包，并设置服务开机自启。

```dockerfile
# erp_dockerfile

FROM centos:centos7.9.2009

RUN rm -rf /etc/yum.repos.d/*
COPY local.repo /etc/yum.repos.d/

COPY yum /opt/yum
RUN yum install java-* -y

COPY app.jar /opt/
CMD java -jar /opt/app.jar

EXPOSE 9999
```

```shell
docker build -t erp-server:v1.0 -f erp_dockerfile .
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
docker build -t explorer-server:v1.0 -f kod_dockerfile .
```