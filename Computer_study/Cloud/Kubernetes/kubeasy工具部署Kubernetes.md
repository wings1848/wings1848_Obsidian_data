---
title: kubeasy工具部署Kubernetes
author: wings1848
date: 2024-05-14 19:21:11
categories: '云计算比赛笔记'
tags: 'kubernetes'
keywords:
---
# 搭建环境

- 安全组相应规则放开
- 网络通信正常
- 系统环境 centos7.9
- 内存12G
- 硬盘100G
- 软件包与镜像等齐全
- 内网文件服务器http://192.168.103.5/FileServer

|主机节点|
| --- | --- |
| 192.168.103.105 | master |
| 192.168.103.175 | node1 |

# 下载镜像
```
[root@master ~]# curl -O http://192.168.103.5/FileServer/ISO/zcloud_1.25.2_pass_base_7.9_v1.0.iso
```

# 安装kubeasy
```
[root@master ~]# mount zcloud_1.25.2_pass_base_7.9_v1.0.iso /mnt/

[root@master ~]# cp -rf /mnt/* /opt/

[root@master ~]# cp /opt/kubeasy /usr/local/bin/
```

# 临时关闭SELinux
```
[root@master ~]# setenforce 0

[root@node1 ~]# setenforce 0
```

# 查看kubeasy的使用帮助
```
[root@master ~]# kubeasy --help
Example:
  [install dependencies package cluster]
  kubeasy install dependencies \
    --host 192.168.1.201,192.168.1.203 \
    --user root \
    --password zcloud@9000 \
    --offline-file /opt/dependencies/packages.tar.gz


  [install k8s cluster]
  kubeasy install kubernetes \
    --master 192.168.1.201 \
    --worker 192.168.1.202,192.168.1.203 \
    --user root \
    --password zcloud@9000 \
    --version 1.25.2 \
    --pod-cidr 10.244.0.0/16 \
    --offline-file /opt/kubecloud.tar.gz
```

# 修改命令参数后执行
修改技巧，vi一个文件，将命令黏贴进去以后修改，再复制出来使用
```
# 修改节点ip,修改密码

[root@master ~]# kubeasy install dependencies \
>     --host 192.168.103.105,192.168.103.175 \
>     --user root \
>     --password Abc@1234 \ 
>     --offline-file /opt/dependencies/packages.tar.gz

[root@master ~]# kubeasy install kubernetes \
>     --master 192.168.103.105 \
>     --worker 192.168.103.175 \
>     --user root \
>     --password Abc@1234 \
>     --version 1.25.2 \
>     --pod-cidr 10.244.0.0/16 \
>     --offline-file /opt/kubecloud.tar.gz
```

# 实时监测kubeasy的日志
```
tailf /var/log/kubeinstall.log
```