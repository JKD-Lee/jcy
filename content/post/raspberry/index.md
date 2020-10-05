+++
date = "2020-03-10"
title = "Raspberry4B安装ArchlinuxARM"
description = "树莓派安装ArchlinuxARM系统..."
tags = [
    "Raspberry",
    "Archlinux",
]

categories = [
    "document",
]
nomenu = "main"
image = "ArchLinuxARM.jpg"
+++

## Raspberry4B下ArchLinuxARM系统的安装配置

#### 前言  
环境及工具  
1.Linux环境  
2.SD卡格式化工具

### 1.配置Linux系统环境

为了获取Linux环境，最简便的方法就是虚拟机安装Linux系统，本人采用[manjaro](https://manjaro.org)系统,是[ArchLinux](https://archlinuxarm.org/)的衍生发行版，继承了ArchLinux的滚动更新特性,用VMWare虚拟机进行安装，由于Manjaro具有LiveCD系统(类似Windows的PE系统)，并且已经安装好了Vmware Tools，这样在实体机与虚拟机之间复制粘贴文件及命令就更加方便，实际上只需要配置好虚拟机，进入LiveCD系统就可以

### 2.制作ArchLinuxARM系统  

1.进入Manjaro的LiveCD系统，插入SD卡，虚拟机识别，选择连接到虚拟机，确定

![ScreenShot_20200310103040.png](https://i.loli.net/2020/03/16/EWRYITKcq9gn5QU.png)  

2.左侧开始菜单打开终端Terminal  

![ScreenShot_20200310103117.png](https://i.loli.net/2020/03/16/4LeFU7YlI6WwqPX.png)  

3.输入`su`切换系统用户  

4.输入`fdisk -l`查看当前插入SD卡的 `/dev/sdX`  

![ScreenShot_20200310103154.png](https://i.loli.net/2020/03/16/74dgU3HRkfabsEj.png)  

5.输入`fdisk /dev/sdX`,X替换为各自显示值，输入`m`查看具体使用命令  
![ScreenShot_20200310103410.jpg](https://i.loli.net/2020/03/16/yKvs6dqzehoYWIf.jpg)  

6.依此输入以下命令，进行格式化分区  
```
1. d     ## 删除分区  
2. n,p,1     ## n创建分区,p设置主分区,1设置第一分区，并enter确认   
3. +100M  
4. t,c     ## 将第一分区改为W95 FAT32 （LBA）  
5. n,p,2     ## n创建第二分区，p设置主分区，enter两次默认确定  
6. w     ## 保存退出  
```
7. 创建挂载系统  
```
mkfs.vfat /dev/sdX1  
mkdir boot  
mount /dev/sdX1 boot  

mkfs.ext4 /dev/sdX2  
mkdir root  
mount /dev/sdX2 root  
```
8. 下载系统并提取到根目录  
```
wget http://mirrors.ustc.edu.cn/archlinuxarm/os/ArchLinuxARM-rpi-4-latest.tar.gz  
bsdtar -xpf ArchLinuxARM-rpi-4-latest.tar.gz -C root  
sync  
```
9. 复制文件到boot分区  
`mv root/boot/* boot`  
10. 卸载分区  
`umount boot root`  
11. 弹出SD卡，插入树莓派开机  

### 3.远程连接ArchLinuxARM并进行系统配置  
1.树莓派供电插网线，使用Xshell远程连接树莓派  
ssh登陆超级用户名：root 密码：root  
主用户名: alarm 密码:alarm  
2.替换国内源  
`nano /etc/pacman.d/mirrorlist `  
添加以下源保存退出    
```
#清华
Server = http://mirrors.tuna.tsinghua.edu.cn/archlinuxarm/$arch/$repo
# 中科大
Server = http://mirrors.ustc.edu.cn/archlinuxarm/$arch/$repo
#成都电子科大
Server = http://mirrors.stuhome.net/archlinuxarm/$arch/$repo

```  
3.初始化pacman密钥环  
`pacman-key --init`  
`pacman-key --populate archlinuxarm`

### 4.完成