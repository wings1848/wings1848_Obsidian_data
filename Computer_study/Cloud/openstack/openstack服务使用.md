# 登录客户端环境

```shell
source /etc/keystone/admin-openrc.sh
```

# Keystone服务使用

---

+ 创建OpenStack域210Demo,包含Engineering与Production项目
```shell
openstack domain create 210Demo

openstack project create --domain 210Demo Engineering
openstack project create --domain 210Demo Production
```

+ 在域210Demo中创建组Devops
```shell
openstack group create --domain 210Demo devops
# 组名使用小写
```

+ Robert用户是Engineering项目的用户（member）与管理员（admin）
+ George用户是Engineering项目的用户（member）
+ William用户是Production项目的用户（member）与管理员（admin）
+ John用户是Production项目的用户（member)

```shell
# 创建用户
openstack user create --domain 210Demo --project Engineering Robert
openstack user create --domain 210Demo --project Engineering George

openstack user create --domain 210Demo --project Production William
openstack user create --domain 210Demo --project Production John
```
```shell
# 添加角色
openstack role add --project Engineering --user Robert member
openstack role add --project Engineering --user Robert admin

openstack role add --project Engineering --user George member


openstack role add --project Production --user William member
openstack role add --project Production --user William admin

openstack role add --project Production --user John member
```

---

# Glance服务使用

+ 使用提供的cirros-0.3.4-x86_64-disk.img镜像上传到OpenStack平台中，命名为deploy-vmlinuz
```shell
# 下载镜像文件
curl -O http://192.168.103.5/FileServer/cirros-0.3.4-x86_64-disk.img

# 创建image
openstack image create --file cirros-0.3.4-x86_64-disk.img deploy-vmlinuz
```

+ 使用提供的AWS内核格式的镜像上传到OpenStack平台中,命名为deploy-vmlinuz
```shell
# 下载镜像文件

# 创建image
openstack image create --disk-format aki --file <file> deploy-vmlinuz
```

# Swift服务使用

+ 创建一个名叫examcontainer的容器
```shell
swift post examcontainer
```

+ 将cirros-0.3.4-x86_64-disk.img镜像上传到examcontainer容器中，并设置分段存放，每一段大小为10M
```shell
swift upload -S 10000000 examcontainer cirros-0.3.4-x86_64-disk.img
```