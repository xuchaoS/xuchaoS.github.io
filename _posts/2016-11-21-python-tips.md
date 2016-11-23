---
layout: post
title:  "python小技巧"
date:   2016-11-21 21:40:16 +0800
categories: python
---
## 1.enumerate函数：
enumerate 函数用于遍历序列中的元素以及它们的下标：

``` python
>>> for i,j in enumerate(('a','b','c')):
        print i,j
         
0 a
1 b
2 c 
```

## 2.Pip 使用其他更新源：

### 临时使用：
可以在使用pip的时候加参数`-i https://pypi.tuna.tsinghua.edu.cn/simple` ,例如：

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple gevent
```
这样就会从清华这边的镜像去安装gevent库。

### 永久修改，一劳永逸：
* Linux下，修改 `~/.pip/pip.conf` (没有就创建一个)， 修改 `index-url`至`tuna`，内容如下：
```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```
* windows下，直接在`user`目录中创建一个pip目录，如：`C:\Users\xx\pip`，新建文件`pip.ini`，内容如下
```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```
