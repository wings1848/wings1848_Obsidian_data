---
title: ADB命令与澎湃OS系统应用精简
author: Wings Butterfly
date: 2024-06-05 13:51:41
categories: Android开发与调试
tags:
  - adb
  - Android
keywords:
---
# ADB命令

[Android Debug Bridge 命令速查](https://weihongbin1.github.io/reference/docs/adb.html)

- 显示已连接的设备
```
adb devices
```

- 列出已经安装的应用程序
```
adb shell pm list packages [options]

options:
	-3 仅列出第三方应用程序
```

- 清理应用程序数据
```
adb shell pm clear <package_name>
```

- 强制停止应用程序
```
adb shell pm force-stop <package_name>
```

- 卸载应用程序
```
adb shell pm uninstall <package_name> [options]

options:
	-k 卸载应用程序但保留数据
	--user <user_id> 指定用户,默认用户id=0
```

- 停用(冻结)应用程序
```
adb shell pm disable-user <package_name>
```

# 小米澎湃OS精简优化

[MIUI系统预装软件列表](https://miuiver.com/wp-content/uploads/miui-pre-installed-software.html)

## 必须保留的应用程序

- 千万不要卸载或停用的应用程序  **(会死机)**
```
com.miui.cloudservice
小米云服务

com.xiaomi.account
小米账户

com.android.updater
系统更新

com.miui.cloudbackup
云备份

com.xiaomi.market
应用市场
```

## 推荐卸载的应用程序

- 广告内置组件
```
com.miui.analytics
Analytics

com.miui.systemAdSolution
智能服务
```

- 快应用框架
```
com.miui.hybrid
快应用服务框架

com.miui.hybrid.accessory
智慧生活
```

```
com.xiaomi.ab
电商助手

com.miui.newhome
内容中心

com.miui.miservice
服务与反馈

com.miui.video
小米视频

com.miui.player
小米音乐

com.android.email
电子邮箱

com.miui.bugreport
用户反馈


```
## 推荐冻结或卸载的应用程序

```
com.miui.personalassistant    
智能助理(负一屏)

com.android.quicksearchbox    
搜索

com.sohu.inputmethod.sogou.xiaomi 
搜狗输入法小米版

com.android.browser    
浏览器

com.miui.carlink    
Carwith车载互联

com.miui.phrase    
常用语

com.android.providers.userdictionary    
用户字典

com.google.android.marvin.talkback    
Android无障碍套件

com.miui.accessibility    
小米无障碍
```