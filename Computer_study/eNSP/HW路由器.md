---
title: HW路由器
author: wings1848
date: 2024-05-20 21:16:47
categories: 网络
tags: 
  - '华为网络设备'
keywords:
---
# HW路由器

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

## 配置路由

+ 配置路由接口ip地址
```
ip address [ip] [mask]
```

+ 配置静态路由

[ip route-static | 华为](https://support.huawei.com/enterprise/zh/doc/EDOC1100064377/ac7caed7)

***目的IP地址和掩码都为0.0.0.0，配置的路由为缺省路由***

```
ip route-static [ip-address] [mask] [nexthop-address]

	ip-address 目的IP地址
	mask 指定IP地址的掩码
	nexthop-address 指定路由的下一跳的IP地址
```

+ 配置浮动路由
```
ip route-static [ip-address] [mask] [nexthop-address] preference [preference-id]

	preference 路由协议的优先级
```

---

## OSPF(开放式最短路径优先协议)
+ 创建ospf进程
```
ospf [process-id] <router-id>

	process-id 进程id,缺省指为1
	router-id 路由编号
```

+  创建OSPF区域，并进入OSPF区域视图
```
area [area-id]
```

+ 配置区域包含的网段
```
network [ip-address] [wildcard mask]
	wildcard mask 通配符掩码(cn称反掩码)
```

---

## VLAN终结

将一个三层以太网接口虚拟划分成多个逻辑子接口

可用于配置单臂路由,只使用一个接口接入用户或网络时,一个接口上需要传输多个VLAN报文,VLANIF无法实现

+ 配置Dot1q终结子接口实现VLAN间的通信
```
dot1q termination vid [low-pe-vid]
```
**同一主接口下的不同子接口不能关联相同的VLAN**

+ 开启ARP广播功能

开启终结子接口的ARP广播功能,允许终结子接口能转发广播报文

可能导致网络的路由发生一次震荡

```
arp broadcast enable
```

## VRRP(虚拟路由冗余协议)

+ 在路由接口中创建VRRP备份组并为备份组指定虚拟IP地址

[vrrp vrid virtual-ip | 华为](https://support.huawei.com/enterprise/zh/doc/EDOC1100096315/95be2af1)
```
vrrp vrid [virtual-router-id] virtual-ip [virtual-address]

	virtual-router-id 指定VRRP备份组号
	virtual-address 指定VRRP备份组的虚拟IP地址
```

+ 在路由接口中配置VRRP备份组的优先级

[vrrp vrid priority | 华为](https://support.huawei.com/enterprise/zh/doc/EDOC1000128396/7eaf2d8a)
```
vrrp vrid [virtual-router-id] priority [priority-value]

	priority-value 设备在VRRP备份组中的优先级,数值越大,优先级越高
```

+ 在路由接口中启用VRRP监视***上行接口***状态实现主备快速切换(通过改变VRRP备份组的优先级)

上行接口:客户端向服务端发送请求的接口

[vrrp vrid track interface | 华为](https://support.huawei.com/enterprise/zh/doc/EDOC1100241872/da21b06d)
```
vrrp vrid [virtual-router-id] track interface [interface-type] [interface-number] {reduced [value-reduced] | increased [value-increased]}

	interface-type interface-number 上行接口类型,接口号
	reduced [value-reduced] 优先级降低的数值
	increased [value-increased] 优先级增加的数值
```

## 网络访问控制安全管理

+ 进入网络访问控制安全管理框架
```
aaa
```
