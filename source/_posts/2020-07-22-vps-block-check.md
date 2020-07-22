---
layout:     post
title:      "VPS 被墙检测"
subtitle:   "VPS 被墙检测"
date:       2020-07-22T23:11:08+08:00
updated:    2020-07-22T23:11:08+08:00
banner_img: /img/post_banner_common.jpg
categories:
  - Linux
tags:
    - VPS
---

遇到该类问题，建议您先检测ip能否ping通：
ping.chinaz.com 请前往该网站，输入自己的ip进行检测 ↓

若：海外通、国内不通，则认定为ip被封锁，可付费更换，
若：国内外都不通，可能是系统内部问题，建议VNC查看状态，或重装系统，
若：都通 但无法访问外网，可能受边境防火墙干扰，建议检查业务是否合规，