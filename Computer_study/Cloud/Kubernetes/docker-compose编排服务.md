---
title: docker-compose编排服务
author: Wings Butterfly
date: 2024-06-11 21:29:47
categories: 
tags: 
keywords:
---
# 构建ERP系统

- 编写docker-compose.yaml完成ERP管理系统的部署，定义mysql、redis、nginx和erp共四个Service，分别使用镜像erp-redis:v1.0、erp-mysql:v1.0、erp-nginx:v1.0和erp-server:v1.0，并将nginx服务的80端口映射到宿主机的8888端口。
```yaml
version: '3'
services:
  erp-mysql:
    container_name: erp-mysql
    restart: always
    image: erp-mysql:v1.0
    ports:
      - 3306:3306
  erp-redis:
    container_name: erp-redis
    restart: always
    image: erp-redis:v1.0
    command: redis-server --requirepass <password> # 数据库密码
    ports:
      - 6379:6379
  erp-server:
    container_name: erp-server
    restart: always
    image: erp-server:v1.0
    ports:
      - 9999:9999
  erp-nginx:
    container_name: erp-nginx
    restart: always
    image: erp-nginx:v1.0
    ports:
      - 8888:80
```