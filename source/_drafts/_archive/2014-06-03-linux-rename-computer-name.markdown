---
layout:     post
title:      "Linux 修改 计算机名"
subtitle:   "Linux 修改 计算机名"
date:       2014-06-03
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - software
---

显示主机名
---
    # hostname

修改主机名(这种方式修改,重启后会失效)
---
    # hostname 新的主机名

重启后不失效的
===
Ubuntu 发行版本:
---
    # vi /etc/hostname (修改该文件内容)

其它发行版本(RedHat) 未测试过
---
    # vi /etc/sysconfig/network，修改HOSTNAME一行为”HOSTNAME=主机名”(没有这行？那就添加这一行吧)，然后运行命令” hostname 主机名”。

修改完后
---
    # vi /etc/hosts (把之前的映射改成新的计算机名)

