# node安装 #node #npm 
[[Windows开发工具与环境#Node.js环境 node]]

***

# npm配置 #npm 

## 修改全局模块和缓存目录(可选)

node安装目录下创建全局模块目录**node_global**和缓存目录**node_cache**

修改全局模块安装与缓存路径  
*管理员身份时*
```
npm config set prefix "绝对路径"

npm config set cache "绝对路径"
```
配置全局模块的用户环境变量和系统环境变量

## cnpm安装 #cnpm 
使用cnpm命令将默认使用国内npm镜像源
```
npm install -g cnpm --registry https://registry.npm.taobao.org
```

使用cnpm需使用管理员模式进入终端，打开脚本运行权限
```
set-ExecutionPolicy RemoteSigned
```

***

# npm命令 #npm 

#清理包缓存    `npm cache clean --force`  
#查看node全局根路径    `npm root -g`  

***

# npm包 #npm 

## hexo #hexo 

安装hexo  ``cnpm install -g hexo-cli``

[[Hexo配置与使用]]