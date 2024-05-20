---
title: Git配置与使用
author: wings1848
date: 2024-02-08 19:36:36
category: 
tags:
---
# Git配置与连接 #git 

配置ssh密钥 #ssh  
`ssh-keygen -t rsa -C "邮箱"`

使用生成的~\.ssh\id_rsa.pub内容在github创建SSH keys

修改`~\.ssh\config`,将连接github的端口换为443  
***22端口被阻断无法使用***
```
Host github.com
Hostname ssh.github.com
Port 443
User git
```

配置用户名  
`git config --global user.name "用户名"`

配置邮箱  
`git config --global user.email "邮箱"`

连接github  
`ssh -T git@github.com`

发现443端口报错,可能是遇到了DNS污染  
修改一下hosts文件将域名`ssh.github.com`指向正确ip即可

***

# Git命令使用 #git 

[Git 教程 | 菜鸟教程](https://www.runoob.com/git/git-tutorial.html)

本地工作区>`.git`本地暂存区>本地版本库>远程仓库

- 初始化Git仓库
```shell
git init <目录名>
```

- 将本地仓库关联到远程仓库
```shell
git remote add <主机名,建议默认为origin> <远程仓库url>
```

- 将远程仓库代码合并到本地
```shell
git pull <远程主机名> <本地分支名>:<远程分支名>
# 本地分支名与远程分支名相同可只填一次
```

- 将工作区内容存入暂存区
```shell
git add .
```

- 将暂存区内容提交到版本库
```shell
git commit -m "提交说明"
```

- 将版本库内容上传到远程仓库并合并
```shell
git push <远程主机名> <本地分支名>:<远程分支名>
```

- 删除远程仓库文件
```shell
git rm -r <文件>
git commit -m "删除说明"
git push <远程主机名> <分支名>
```

- clone仓库
```shell
git clone git@github.com:<文件地址>
```