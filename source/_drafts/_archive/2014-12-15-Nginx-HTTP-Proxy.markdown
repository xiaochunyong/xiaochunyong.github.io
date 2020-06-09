---
layout:     post
title:      "Nginx 搭建 HTTP 代理服务器"
subtitle:   "Nginx 搭建 HTTP 代理服务器"
date:       2014-12-15
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - proxy, nginx, gfw, google, youtube, facebook
---
Nginx 只支持HTTP协议正向代理

    server {
        listen 8080;
        resolver 8.8.8.8;
        location /{
            proxy_pass http://$http_host$request_uri;
        }
    }
