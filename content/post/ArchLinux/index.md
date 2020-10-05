---
date: 2020-02-18
title: Win10下安装Archlinux子系统
description : "Win10下WSL安装Archlinux子系统..."
weight: 10
tags : ["WIN10"]
categories : ["document"]
image : "Archlinux.jpg" 
---

## <center>在Win10系统下，最简单安装ArchLinux子系统</center>

### 1.启用适用于Linux的Windows子系统

控制面板-程序-启用或关闭Windows功能-启用适用于Linux的Windows子系统-重启电脑
![2020-02-18-16-43-49-image.png](https://i.loli.net/2020/02/18/QAg3TrbGL6IpelC.png)

### 2.下载安装ArchLinux
1.进入[ArchWSL](https://github.com/yuk7/ArchWSL/releases),下载Arch.zip,解压至一个具有写权限的普通文件夹(注：Program Files文件夹不具备写的权限)  
2.双击解压出的Arch.exe.执行安装  
![3](https://i.loli.net/2020/02/18/KeRx7vNom4Olj5g.png)
### 3.配置ArchLinux
1.使用如下命令，初始化keyrin(使用pacman必须执行此命令)  
``` 
sudo pacman-key --init
sudo pacman-key --populate
```
2.打开Arch.exe根目录下的rootfs\etc\pacman.d\mirrorlist,将##China及以下的server全部剪切至## Worldwide之前，并将server之前的#全部删除，保存退出(换源提升下载速度)

3.执行`pacman -Syyu`命令更新组件  

![3](https://i.loli.net/2020/02/18/izQxGDL17bTCyRH.png)

### 最后，请尽情享用吧





#### 注：根据官方提示，Win10系统版本号需在1709之后
