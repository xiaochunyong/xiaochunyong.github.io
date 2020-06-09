---
layout:     post
title:      "Linux Terminal Tab键忽略大小写自动补全"
subtitle:   "Linux Terminal Tab键忽略大小写自动补全"
date:       2014-06-02
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - software
---

系统配置文件为/etc/inputrc

修改个人配置文件:

    echo "set completion-ignore-case on" >> ~/.inputrc

重启终端
