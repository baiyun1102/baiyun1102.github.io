---
layout: post
title:  "SecureCRT下文件名显示乱码（问号）的解决办法"
date:   2019-03-14 10:05:00

categories: other
tags: linux
author: "choi"
---



**今天在用SecureCRT登录服务器的时候发现，ls命令查看文件列表。中文文件名显示为问号，如下图：**
![](../../assets/images/pictures/2019-03-14-SecureCRT/20190314132334.png)  

网上查了查应该修改一下lang这个变量。步骤如下：

- 1、先用locale命令查看，发现配置的字符集不对
- 2、修改用户变量：vi ~/.bash_profile 填下如下内容：

```
LANG=zh_CN.UTF-8
LC_CTYPE="zh_CN.UTF-8"
LC_NUMERIC="zh_CN.UTF-8"
LC_TIME="zh_CN.UTF-8"
LC_COLLATE="zh_CN.UTF-8"
LC_MONETARY="zh_CN.UTF-8"
LC_MESSAGES="zh_CN.UTF-8"
LC_PAPER="zh_CN.UTF-8"
LC_NAME="zh_CN.UTF-8"
LC_ADDRESS="zh_CN.UTF-8"
LC_TELEPHONE="zh_CN.UTF-8"
LC_MEASUREMENT="zh_CN.UTF-8"
LC_IDENTIFICATION="zh_CN.UTF-8"
LC_ALL=
```

![](../../assets/images/pictures/2019-03-14-SecureCRT/20190314133937.png)


- 3、断开SecureCRT，重新登录。用locale命令查看，发现配置已经生效。使用ls命令，文件名称已经正常显示。
