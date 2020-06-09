---
layout:     post
title:      "如何强制刷新本地DNS缓存？"
subtitle:   "Ub如何强制刷新本地DNS缓存？"
date:       2014-11-20

banner_img: /img/post_banner_common.jpg

tags:
    - software
---

### Windows用户
* 1.在开始菜单——运行中输入CMD
* 2.在命令提示符中输入ipconfig/flushdns回车即可刷新本地DNS缓存。

### Mac OS用户
* 在终端中键入sudo killall -HUP mDNSResponder即可刷新本地DNS缓存。
* dscacheutil -flushcache

### Linux用户
键入/etc/init.d/nscd restart命令重启nscd daemo即可刷新本地DNS缓存。

### 路由器用户
进入路由器管理界面，重启路由器即可刷新路由器DNS缓存。
