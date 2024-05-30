---
title: docker命令
author: wings1848
date: 2024-04-23 22:49:23
categories: '开发工具配置与使用'
tags: 
  - 'docker'
keywords:
---
[Docker 命令大全 菜鸟教程](https://www.runoob.com/docker/docker-command-manual.html)

# 容器操作

+ 列出所有容器

[Docker ps 命令 菜鸟教程 ](https://www.runoob.com/docker/docker-ps-command.html)
```
docker ps [options]

options：
	-a 显示所有容器
```

---

# 容器生命周期管理

+ 使用容器镜像创建一个新的容器，并运行一个命令

[Docker run 命令  菜鸟教程 ](https://www.runoob.com/docker/docker-run-command.html)
```
docker run [options] <image> [command]
```

+ 删除容器

[Docker rm 命令 菜鸟教程 ](https://www.runoob.com/docker/docker-rm-command.html)
```
docker rm [options] <container>

options:
	-f 强制删除运行中的容器
```

+ 停止运行中的容器
```
docker stop [options] <container>
```

---

# 本地镜像管理

+ 列出镜像

[Docker images 命令 菜鸟教程](https://www.runoob.com/docker/docker-images-command.html)
```
docker images [options] [repository[:tag]]
```

+ 从.tar文件导入镜像

[Docker load 命令 菜鸟教程 ](https://www.runoob.com/docker/docker-load-command.html)
```
docker load [options]

options:
	-i 指定导入的文件
```

+ 构建镜像

[Docker build 命令 菜鸟教程](https://www.runoob.com/docker/docker-build-command.html)
```
docker build [options] <path | url>

options:
	-f 指定要使用的Dockerfile路径
	--tag | -t  镜像的名字及标签,name:tag或者name格式
```

+ 删除本地镜像

[Docker rmi 命令 菜鸟教程](https://www.runoob.com/docker/docker-rmi-command.html)
```
docker rmi [options] <image> [image...]

options:
	-f 强制删除
```