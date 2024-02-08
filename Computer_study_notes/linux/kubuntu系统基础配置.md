# Kubuntu折腾之旅

记录一下kubuntu用做日常使用的折腾之路，对于其他debian系Linux系统的日常使用有一定参考价值

因为个人的笔记本是很老旧的机子，于是尝试使用linux内核系统的kubuntu系统

## 下载安装kubuntu

<https://kubuntu.org/getkubuntu/>

## 配置apt软件源

下载安装完成kubuntu之后，因为ubuntu自带apt软件包管理器的软件源默认是国外源，为提升软件下载速度，需要修改为国内软件源。

```sudo apt-get install vim```安装vim

命令```sudo vim /etc/apt/sources.list```使用vim编辑器编辑sources.list文件


[北京外国语大学开源软件镜像站ubuntu镜像站使用帮助](https://mirrors.bfsu.edu.cn/help/ubuntu/)

## 更新apt软件源和软件

配置完软件源后，输入命令```sudo apt-get update```更新软件源，再输入命令```sudo apt-get upgrade```更新所有软件

## 添加铜豌豆Linux软件库

铜豌豆软件库官网<https://www.atzlinux.com/allpackages.htm>

添加铜豌豆软件库，方便安装一些日常常用软件，请逐行运行以下命令

```sudo apt-get install wget -y

sudo curl -O atzlinux-v11-archive-keyring_lastest_all.deb https://www.atzlinux.com/archive/pool/main/a/atzlinux-keyring/atzlinux-v11-archive-keyring_lastest_all.deb

sudo apt-get install ./atzlinux-v11-archive-keyring_lastest_all.deb -y

sudo rm -rf ./atzlinux-v11-archive-keyring_lastest_all.deb

sudo apt-get update

sudo apt-get upgrade
```

## 安装日常常用软件命令

安装搜狗输入法 `sudo apt-get install sogoupinyin`
安装输入法之后需要重启电脑，再手动从右下角fcitx5输入法管理器里设置一下顺序

安装软件使用 `sudo apt-get install` 空格后加上软件包名加 `-y`

安装QQ `sudo apt-get install linuxqq`

安装腾讯会议 `sudo apt-get install wemeet`

安装哔哩哔哩 `io.github.msojoces.bilibili`

安装网易云音乐替代品 `yesplaymusic`

安装vs code编辑器 `code`

安装微信 `electronic-wechat`

安装钉钉 `com.alibabainc.dingtalk`

QQ音乐 `qqmusic`

腾讯视频 `tenvideo-universal`

wps办公 `wps-office`

zoom视频会议 `zoom`

磁盘管理工具 `gparted`