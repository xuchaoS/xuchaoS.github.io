---
layout: post
title:  "Linux基础学习"
date:   2017-03-30 17:48:00 +0800
categories: Linux
---
## 启动级别

* 0：停机
* 1：单用户形式，只root进行维护
* 2：多用户，不能使用net file system
* 3：完全多用户
* 5：图形化
* 4：安全模式
* 6：重启

其实，可以通过查看`/etc/rc.d/`中的`rc*.d`的文件来对比理解，不同的目录中包含的命令是不同的  
`init 0`，对应的系统会运行，`/etc/rc.d/rc0.d`里指定的程序
S00killall、 S01halt  
这两个都表示为终止进程。故init 0是用于表示关机的

## Shell终端