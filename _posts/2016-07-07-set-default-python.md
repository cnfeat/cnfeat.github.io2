---
layout: post
title: Debian下修改默认python版本
date: 2016-07-07
categories: blog
tags: [技术]
description: 思考，从写作开始
---

缘起于近来用debian，系统内置python两个版本，默认是python2.7，但python3已是趋势，所以想将3设为默认。

首先，运行ls命令找出python所在所有路径：

> $ ls /usr/bin/python*

输出一般类似于：
> /usr/bin/python  /usr/bin/python2  /usr/bin/python2.7  /usr/bin/python3  /usr/bin/python3.4  /usr/bin/python3.4m  /usr/bin/python3m

检查python的版本
> python --version

然后有3种方法可以实现：

1. 第一种是对当前用户生效的，命令很简单：

> alias python='/usr/bin/python3.4'

然后为了让其生效，可以重新登录或者重启.bashrc文件

> . ~/.bashrc

再用 python --version 确认操作生效

2. 系统层面改变python版本

update-alternatives 命令是主角。首先列出可选python：

> update-alternatives --list python

如果出现“update-alternatives: error: no alternatives for python” 则表示没有python版本被识别为可选，所以需要我们手动添加：

> update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1

> update-alternatives --install /usr/bin/python python /usr/bin/python3.4 2

没有报错即可，另外记得将python后的数字换成你自己系统的相应python版本号。再用 python --version 和 update-alternatives --list python 两条命令进行验证。

3. 这条方法比较暴力，直接卸载你不想再用的python版本，不过一般还是不用这么决绝啦。

> update-alternatives --remove python /usr/bin/python2.7
