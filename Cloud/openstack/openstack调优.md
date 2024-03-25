# my.cnf调优

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

# memcached调优

修改/etc/sysconfig/memcached

+ 将内存占用大小设置为512MB
```
MAXCONN="2048"
```
+ 调整最大连接数参数为2048
```
CACHESIZE="256"
```
+ 调整Memcached的数据摘要算法（hash）为md5