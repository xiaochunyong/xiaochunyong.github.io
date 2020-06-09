---
layout:     post
title:      "配置 Nginx 支持 WordPress的固定链接"
subtitle:   "配置 Nginx 支持 WordPress的固定链接"
date:       2014-05-31
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - nginx, wordpress
---

修改nginx配置文件
---
    location / {
        if (-f $request_filename/index.html){
            rewrite (.*) $1/index.html break;
        }

        if (-f $request_filename/index.php){
            rewrite (.*) $1/index.php;
        }

        if (!-f $request_filename){
            rewrite (.*) /index.php;
        }
    }
