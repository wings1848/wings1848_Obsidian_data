查询参数
```
sysctl -a |grep [keyword]
```

调优后配置文件（仅参考）
```/etc/sysctl.conf
# 防SYN洪水攻击
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_max_syn_backlog = 1024
# 重试发送SYN+ACK 数据包的次数
net.ipv4.tcp_synack_retries = 2


# 关闭ipv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1

# 避免放大攻击
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0

# 主机禁ping
net.ipv4.icmp_echo_ignore_all = 1

# 进程的最大虚拟内存区域数量
vm.max_map_count=262144

# 打开文件描述符的数量
fs.file-max=6553560

# 单个消息队列最大字节数
kernel.msgmnb = 65536

# 消息队列中单个消息的最大字节数
kernel.msgmax = 65536

# 套接字缓冲区大小
net.core.rmem_default = 262144
net.core.rmem_max = 366592
net.core.wmem_default = 262144
net.core.wmem_max = 366592

# tcp连接的关闭时限
net.ipv4.tcp_fin_timeout = 30

# tcp连接的空闲超时时间,设为20min
net.ipv4.tcp_keepalive_time = 1200

# 禁用交换空间
vm.swappiness = 0

# 脏数据回刷调优（高性能）
vm.dirty_background_bytes = 30
dirty_ratio= 60
vm.dirty_writeback_centisecs = 1000
vm.dirty_expire_centisecs = 6000
```

永久保存内核参数
```
sysctl -p
```