---
layout:     post
title:      "Linux 系统个性化"
subtitle:   "Linux 系统个性化"
date:       2014-06-02
banner_img: /img/post_banner_common.jpg
index_img: /img/index/Linux.jpeg
categories:
  - Linux
tags:
    - linux
---

系统配置文件为/etc/inputrc

修改个人配置文件:

    echo "set completion-ignore-case on" >> ~/.inputrc

重启终端

启动Linux 控制台后, 可以看到Shell提示符
类似 ```[ely@localhost ~]$``` 
 `#` 表示超级用户(root用户) `$` 表示超级用户(普通用户)
常用命令
```
# hostname // 显示主机名
# hostname <myhostname> // 修改主机名, 重启失效
# vi /etc/hostname // 修改主机名(ubuntu), 重启不失效(不同发行版可能有差别)
```
