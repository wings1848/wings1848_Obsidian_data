# Mysql调优

***查询/etc/my.cnf的参数***
```shell
mysql -uroot -p000000 -e "show variables;" |grep <关键字>
```

参数列表

+ 设置mysql数据库支持大小写
```
lower_case_table_names=1
```

+ 设置mysql数据库缓存innodb表的索引，数据，插入数据时的缓冲为4G

```
innodb_buffer_pool_size=4G
```

+ 设置mysql数据库的log buffer为64MB
```
innodb_log_buffer_size=64MB
```


+ 设置mysql数据库的redo log大小为256MB
```
innodb_log_file_size=256MB
```

+ 设置mysql数据库的redo log文件组为2
```
innodb_log_files_in_group=2
```
+ 重启mysql
```shell
systemctl restart mysqld
systemctl restart mariadb
```

---
# Memcached调优

修改/etc/sysconfig/memcached

+ 将内存占用大小设置为512MB
```
CACHESIZE="256"
```

+ 调整最大连接数参数为2048
```
MAXCONN="2048"
```

+ 调整Memcached的数据摘要算法（hash）为md5
```
hash_algorithm=md5
```

+ 需重启memcached
```
systemctl restart memcached
```

---

# Nova调优

+ 修改调度器规则采用缓存调度器，缓存主机信息，提升调度时间
```shell
vi /etc/nova/nova.conf

 # 使用/查询
```
```.conf
/[schedule

driver=caching_scheduler
```

---

# Dashboard调优

+ 使得登录Dashboard平台的时候不需要输入域名
```shell
vim /etc/openstack-dashboard/local_settings
```
```
/SESSION_

SESSION_ENGINE = 'django.contrib.sessions.backends.file'
```

+ 将Dashboard中的Djingo数据修改为存储在文件中
```
/OPENSTACK

OPENSTACK_KEYSTONE_MULTIDOMAIN_SUPPORT = False
```

---

+ 在计算节点，对块存储进行扩容操作,在计算节点再分出一个5G的分区，加入到cinder块存储的后端存储中去,***提交compute节点***

[[ChinaSkills云计算应用openstack搭建#分区]]
```
vgdisplay

vgextend cinder-volumes /dev/vdb4

vgdisplay
```