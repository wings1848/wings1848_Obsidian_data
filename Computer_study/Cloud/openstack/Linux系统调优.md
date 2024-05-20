# 脏数据回写

+ 系统默认脏数据30秒后会回写磁盘，修改系统配置文件，将回写磁盘的时间临时调整为60秒

```shell
sysctl -a | grep vm

vi /etc/sysctl.conf
```
```
vm.dirty_expire_centisecs = 6000
```
```shell
sysctl -p
```