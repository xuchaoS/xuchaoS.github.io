---
layout: post
title:  "Linux小技巧"
date:   2016-11-23 15:02:00 +0800
categories: linux
---
## ubuntu修改启动顺序：
`/etc/default/grub`中的`GRUB_DEFAULT`，修改完`sudo update-grub`

## 流量监控：
`sar -n DEV 1 3600`

## 改时间

```
date -s **/**/**
date -s **：**：**
```

## 改时区
`tzselect`

## 删除大量文件
`find . -name *** -delete` 

## 添加新硬盘：
* `fdisk -l` 查看新添加的硬盘
* `fdisk /dev/sd* n p \n \n w` 创建硬盘分区
* `mkfs -t ext3 /dev/sd*1` 格式化分区
1. 手动挂载：使用 `mount /dev/sd*1 /` 要挂载的目录（自己自定义）
访问时：`cd /` 挂载的目录,即可对其进行存储和访问
2. 自动挂载：修改 `/etc/fstab` 即可
使用 `vim /etc/fstab` 打开配置的文件，然后将下面的一行文字添加即可

``` shell
/dev/sdb1       /media      ext3    defaults       0       1
#其中 /media 这个挂载的目录你自己设置即可
```
## 重新挂载
`mount -o remount rw /`  
重新挂载根目录

## 修复硬盘
`fsck`

## 修改网络配置
```
nmcli con show #列出目前可用的网络连接
nmcli dev status #检查可用设备
nmcli con add type ethernet con-name "connection-name" ifname "interface-name" #添加动态以太网连接
nmcli con up "connection-name" #激活以太网连接
nmcli con add type ethernet con-name "connection-name" ifname "interface-name" ip4 "address/mask" gw4 "address" #添加静态以太网连接
```

## 定时任务
### crontab的文件格式
分 时 日 月 星期 要运行的命令

第1列分钟0～59  
第2列小时0～23（0表示子夜）  
第3列日1～31  
第4列月1～12  
第5列星期0～7（0和7表示星期天）  
第6列要运行的命令  

如``0,15,30,45 18-06 * * * /bin/echo `date` > dev/tty1``

### crontab命令
```
crontab -l # 列出当前的定时任务
crontab -r # 删除定时任务
crontab "filename" # 添加定时任务

```

