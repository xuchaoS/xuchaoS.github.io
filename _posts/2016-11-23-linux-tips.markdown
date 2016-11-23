---
layout: post
title:  "Linux小技巧"
date:   2016-11-23 15:02:00 +0800
categories: linux
---
## 1. ubuntu修改启动顺序：
`/etc/default/grub`中的`GRUB_DEFAULT`，修改完`sudo update-grub`
## 2. 流量监控：
`sa r -n DEV 1 3600`
## 3. 改时间
```
date -s **/**/**
date -s **：**：**
```
## 4. 改时区
`tzselect`
## 5. 删除大量文件
`find . -name *** -delete` 
## 6. 添加新硬盘：
* `fdisk -l` 查看新添加的硬盘
* `fdisk /dev/sd* n p \n \n w` 创建硬盘分区
* `mkfs -t ext3 /dev/sd*1` 格式化分区
1. 手动挂载：使用 `mount /dev/sd*1 /` 要挂载的目录（自己自定义）  
访问时：`cd /` 挂载的目录,即可对其进行存储和访问
2. 自动挂载：修改 `/etc/fstab` 即可  
使用 `vim /etc/fstab` 打开配置的文件，然后将下面的一行文字添加即可  
```
/dev/sdb1       /media      ext3    defaults       0       1
```
其中 `/media` 这个挂载的目录你自己设置即可
