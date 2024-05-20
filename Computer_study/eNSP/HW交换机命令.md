---
title: HW交换机命令
author: wings1848
date: 2024-04-22 10:29:02
categories: 网络
tags:
  - '华为网络设备'
keywords:
---
+ 从用户视图进入系统视图
```
sys
或
system-view
```

+ 设置交换机主机名
```
sysname [name]
```

+ 关闭弹出日志
```
undo info-center enable
或
undo terminal monitor
```

+ 进入接口
```
int [port]
或
interface [port]
```

+ 退出接口
```
q
或
quit
```

+ 保存配置(用户视图)
```
save
```

+ 查询
```
dis [options]
或
display [options]
```

---

# 交换机

## 交换机接口配置

+ 配置接口链路类型
```
port link-type [options]

options:
	access 主要用于交换机连接PC
	trunk 交换机连接交换机
```

+ 配置Access接口的缺省vlan并加入这个vlan
```
port default vlan []
```

+ 设置Trunk类型的接口加入的vlan,让vlan的报文通过Trunk接口
```
port trunk allow-pass vlan [vlan-id]
```

+ 创建临时接口组
```
port-group group-member [] to []
```

+ 进入Vlanif接口(基于Vlan的三层逻辑接口)视图
```
int Vlanif [vlan-id]
```

---

## 交换机接口安全

+ 打开接口安全功能

*学习到的MAC变为安全动态MAC,只有允许的MAC地址的数据包才能被转发*

```
port-security enable
```

+ 开启接口Sticky MAC功能

[port-security mac-address sticky参考 - 华为 (huawei.com)](https://support.huawei.com/enterprise/zh/doc/EDOC1100064377/b27717f3)

```
port-security mac-address sticky
```

+ 配置接口Sticky MAC表
```
port-security mac-address sticky [mac] [vlan]
```

---

## 交换机生成树与快速生成树

+ 开启stp服务
```
stp enabled
```

+ 配置交换设备生成树协议的工作模式
```
stp mode [options]

options:
	stp 生成树协议
	rstp 快速生成树协议
```

+ 配置设备stp服务的优先级

```
stp priority [priority-id]
```
或
```
undo stp priority
stp root [options]

options:
	priority 优先的
	secondary 次要的
```

+ 启用stp边缘接口
```
stp edged-port enable
```

---

## VLANIF

[配置VLANIF接口实现不同VLAN间的互通](https://support.huawei.com/enterprise/zh/doc/EDOC1100278264/fefe3be5)

VLANIF接口是一种三层的逻辑接口，能实现不同VLAN间，不同网段的用户进行三层互通。

+ 创建并进入VLANIF接口
```
int vlanif [vlan-id]
```
