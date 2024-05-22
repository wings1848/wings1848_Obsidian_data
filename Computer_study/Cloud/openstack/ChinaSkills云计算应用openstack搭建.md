# 软件包

已知包含国基北盛openstack,未验证是否有一道云openstack  
ChinaSkills云计算应用为一道云openstack
```
# centos软件包
http://mirrors.douxuedu.com/competition/CentOS-7-x86_64-DVD-2009.iso

# iaas软件包
http://mirrors.douxuedu.com/competition/chinaskills_cloud_iaas_v2.0.1.iso
```

# 搭建环境

- 安全组相应规则放开
- 网络通信正常
- 系统环境 centos7.9
- 内存12G
- 硬盘最少50G
- compute节点挂载着50G数据盘
- yum软件包与镜像等齐全

|主机节点|
| --- | --- |
| 10.24.17.240 | controller |
| 10.24.17.229 | compute |


```shell
[root@wings-1 ~]# hostnamectl set-hostname controller
[root@wings-1 ~]# bash

[root@wings-2 ~]# hostnamectl set-hostname compute
[root@wings-2 ~]# bash
```

# 修改hosts
```shell
[root@controller ~]# vi /etc/hosts
10.24.17.240 controller
10.24.17.229 compute
```

# 配置免密登录
```shell
[root@controller ~]# ssh-keygen 

[root@controller ~]# ssh-copy-id compute

[root@controller ~]# scp /etc/hosts compute:/etc/hosts

[root@compute ~]# ssh-copy-id

[root@compute ~]# ssh-copy-id controller
```

# 配置yum源
```shell
[root@controller ~]# rm -rf /etc/yum.repos.d/*

[root@compute ~]# rm -rf /etc/yum.repos.d/*

[root@controller ~]# vi /etc/yum.repos.d/http.repo

# 网络源,172.128.10.10为文件服务器ip
[centos]
baseurl=http://172.128.10.10/centos7.9/
gpgcheck=0
enabled=1
[iaas]
baseurl=http://172.128.10.10/iaas/iaas-repo/
gpgcheck=0
enabled=1

[root@controller ~]# scp /etc/yum.repos.d/http.repo compute:/etc/yum.repos.d/http.repo
```

# 分区
```shell
[root@compute ~]# umount /mnt/

[root@compute ~]# vi /etc/fstab
# 删最后一行/mnt的配置,否则搭建可能出错

[root@compute ~]# fdisk /dev/vdb
# 分区,提前分出vdb4备用

[root@compute ~]# lsblk 
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
vda    253:0    0  100G  0 disk 
└─vda1 253:1    0  100G  0 part /
vdb    253:16   0   50G  0 disk 
├─vdb1 253:17   0   10G  0 part 
├─vdb2 253:18   0   10G  0 part 
├─vdb3 253:19   0   10G  0 part 
└─vdb4 253:20   0    5G  0 part
```

# 下载部署的脚本和一些工具
```shell
[root@controller ~]# yum install openstack-iaas vim unzip -y

[root@compute ~]# yum install openstack-iaas -y
```

# 修改部署脚本
```shell
[root@controller ~]# vi /etc/openstack/openrc.sh

# 快捷键取消注释
Ctrl+g
Shift+v
x

# 快速填充password
:%s/PASS=/PASS=000000

# 需修改的配置，其他默认
HOST_IP=10.24.17.240

#Controller HOST Password. example:000000
HOST_PASS=Abc@1234

#Controller Server hostname. example:controller
HOST_NAME=controller

#Compute Node Manager IP. example:x.x.x.x
HOST_IP_NODE=10.24.17.229

#Compute HOST Password. example:000000
HOST_PASS_NODE=Abc@1234

#Compute Node hostname. example:compute
HOST_NAME_NODE=compute

#--------------------Chrony Config-------------------##
#Controller network segment IP.  example:x.x.0.0/16(x.x.x.0/24)
network_segment_IP=all

#--------------------Keystone Config------------------##
#Password for Keystore admin user. exmaple:000000
DOMAIN_NAME=demo

#Cinder Block Disk. example:md126p3
BLOCK_DISK=vdb1


#The NODE Object Disk for Swift. example:md126p4.
OBJECT_DISK=vdb2

#The NODE IP for Swift Storage Network. example:x.x.x.x.
STORAGE_LOCAL_NET_IP=10.24.17.229

#The NODE Object Disk for Manila. example:md126p5.
SHARE_DISK=vdb3
```

# 修改脚本bug
```shell
[root@controller ~]# vi /usr/local/bin/iaas-pre-host.sh 

/yum u
# 注释yum upgrade -y

[root@controller ~]# scp /etc/openstack/openrc.sh compute:/etc/openstack/openrc.sh

[root@controller ~]# scp /usr/local/bin/iaas-pre-host.sh compute:/usr/local/bin/iaas-pre-host.sh
```

# 执行openstack初始化脚本
```shell
[root@controller ~]# source /etc/openstack/openrc.sh
[root@compute ~]# source /etc/openstack/openrc.sh
[root@controller ~]# iaas-pre-host.sh
[root@compute ~]# iaas-pre-host.sh
# 重连主机
```

# 搭建openstack
```shell
# 按顺序执行脚本部署openstack
[root@controller ~]# iaas-install-mysql.sh

[root@controller ~]# iaas-install-keystone.sh

[root@controller ~]# iaas-install-glance.sh

[root@controller ~]# iaas-install-placement.sh

[root@controller ~]# iaas-install-nova-controller.sh

[root@compute ~]# iaas-install-nova-compute.sh 

[root@controller ~]# iaas-install-neutron-controller.sh

[root@compute ~]# iaas-install-neutron-compute.sh

[root@controller ~]# iaas-install-dashboard.sh

[root@controller ~]# iaas-install-swift-controller.sh 

[root@compute ~]# iaas-install-swift-compute.sh

[root@controller ~]# iaas-install-cinder-controller.sh

[root@compute ~]# iaas-install-cinder-compute.sh

[root@controller ~]# iaas-install-cloudkitty.sh

[root@controller ~]# iaas-install-heat.sh
```

# 安装openstack命令补全工具

```
[root@controller ~]# yum install bash-c*
[root@controller ~]# openstack complete >> .bashrc
[root@controller ~]# source .bashrc
[root@controller ~]# bash
```