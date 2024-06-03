---
title: DockerFile指令
author: wings1848
date: 2024-04-22 12:17:05
categories: '开发工具配置与使用'
tags: 
  - 'docker'
keywords:
---
# DockerFile文件中的指令

[dockerfile | 菜鸟教程](https://www.runoob.com/docker/docker-dockerfile.html)

-  指定基础镜像，用于后续的指令构建
```dockerfile
from <image>
```

- 在构建过程中在镜像中执行命令
```dockerfile
run <command>
```

- 将文件或目录复制到镜像中
```dockerfile
copy <src> <dest>
```

- 将文件、目录或远程URL的文件复制到镜像中,tar压缩文件会进行解压
```dockerfile
add <src> <dest>
```

- 在容器内部设置环境变量
```dockerfile
env <key>=<value>
```

- 声明容器运行时监听的网络端口
```dockerfile
expose <port>
```

- 指定容器创建时的默认命令(可被覆盖)
```dockerfile
cmd <command>

cmd ["param"]
```