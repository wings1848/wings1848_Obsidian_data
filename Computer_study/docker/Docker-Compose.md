---
title: Docker-Compose
author: wings1848
date: 2024-06-03 19:09:03
categories: '开发工具配置与使用'
tags: 'docker'
keywords:
---
# docker-compose命令

[命令说明 · Docker](https://docker-practice.github.io/zh-cn/compose/commands.html#compose-%E5%91%BD%E4%BB%A4%E8%AF%B4%E6%98%8E)

命令格式
```shell
docker compose [options] [COMMAND] [ARGS...]

options:
	-f 指定使用的compose模板文件
	-p 指定项目名称,默认使用目录名称作为项目名
```

- 构建（重新构建）项目中的服务容器
```shell
docker compose build [options] [SERVICE...]
```

- 启动容器应用服务
此命令会自动构建镜像,（重新）创建服务,启动服务,并关联服务相关容器,
默认情况启动的容器都在前台
```shell
docker compose up [options] [SERVICE...]
```

- 停止所启动的容器应用服务，并移除容器网络
```shell
docker compose down
```

- 停止已经处于运行状态的容器应用服务，但不删除
```shell
docker compose stop [options] [SERVICE...]
```

- 启动已经存在的服务容器
```shell
docker compose start [SERVICE...]
```

---

# docker-compose.yaml模板

[Compose 模板文件 · Docker](https://docker-practice.github.io/zh-cn/compose/compose_file.html)

```yaml
version: "3" # 指定模板使用的版本
services:
  <server_name>: # 服务名称
    container_name: # 启动后容器服务的名称
    restart: # 容器退出后的重启策略,always为始终重启
    build: # Dockerfile的路径,会进行自动构建后使用这个镜像
    image: # 使用的镜像(名称或id)
    command: # 覆盖容器启动后默认执行的命令
    ports: # 暴露端口信息,宿主端口:容器端口格式
```