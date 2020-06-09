---
layout:     post
title:      "install wordpress"
subtitle:   "install wordpress"
date:       2016-09-20

banner_img: /img/post_banner_common.jpg

tags:
    - software
---

* 目录
{:toc}

> Tip: 配置文件中的部分目录需要自行创建，可根据错误提示创建。

# 安装Nginx
```
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:nginx/stable
sudo apt-get update
sudo apt-get install nginx
```


# 安装hhvm
```
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0x5a16e7281be7a449
# 国外
#sudo add-apt-repository "deb http://dl.hhvm.com/ubuntu $(lsb_release -sc) main"
# 国内
sudo add-apt-repository "deb http://dl.hiphop-php.com/ubuntu trusty main"

sudo apt-get update
sudo apt-get install hhvm
```
安装完后默认已启动。

# 下载WordPress
```
wget https://wordpress.org/latest.zip
unzip latest.zip
mv wordpress /var/www/blog.ely.me
```

# 配置
## 不支持HTTPS
### Nginx配置
```
server {
  listen 80;
  server_name blog.ely.me;

  location /nginx_status {
    allow 127.0.0.1;
    deny all;
    stub_status on;
  }

  root /var/www/blog.ely.me;

  access_log /var/log/nginx/blog.ely.me/access.log;
  error_log /var/log/nginx/blog.ely.me/error.log;


  if (-f $document_root/system/maintenance.html) {
    rewrite ^(.*)$ /system/maintenance.html break;
  }

  location ~ (/assets|/system|/avatar.png|/favicon.ico|/*.txt) {
    access_log        off;
    expires           14d;
    gzip_static on;
    add_header  Cache-Control public;
  }

  location / {
    if ($host != 'blog.ely.me') {
      rewrite ^/(.*)$ https://blog.ely.me/$1 permanent;
    }
    try_files $uri $uri/ /index.php?$args;
    index index.php;
    include hhvm.conf;
    proxy_redirect     off;
    proxy_set_header   Host $http_host;
    proxy_set_header   X-Forward-For $remote_addr;
    proxy_set_header   Host $host;
    proxy_set_header   X-Forwarded-Host $host;
    proxy_set_header   X-Forwarded-Server $host;
    proxy_set_header   X-Real-IP        $remote_addr;
    proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
    proxy_buffering    on;
    proxy_http_version 1.1;
    proxy_set_header   Upgrade $http_upgrade;
    proxy_set_header   Connection "Upgrade";
    proxy_set_header   X-Forwarded-Proto https;
    gzip on;
  }

}
```


## 支持HTTPS
### 生成自认证证书

执行如下脚本生成自认证证书，并按提示将证书复制到指定目录

```
#!/bin/sh
read -p "Enter your domain [www.example.com]: " DOMAIN

echo "Create server key..."

openssl genrsa -des3 -out $DOMAIN.key 1024

echo "Create server certificate signing request..."

SUBJECT="/C=US/ST=Mars/L=iTranswarp/O=iTranswarp/OU=iTranswarp/CN=$DOMAIN"

openssl req -new -subj $SUBJECT -key $DOMAIN.key -out $DOMAIN.csr

echo "Remove password..."

mv $DOMAIN.key $DOMAIN.origin.key
openssl rsa -in $DOMAIN.origin.key -out $DOMAIN.key

echo "Sign SSL certificate..."

openssl x509 -req -days 3650 -in $DOMAIN.csr -signkey $DOMAIN.key -out $DOMAIN.crt

echo "TODO:"
echo "Copy $DOMAIN.crt to /etc/nginx/ssl/$DOMAIN.crt"
echo "Copy $DOMAIN.key to /etc/nginx/ssl/$DOMAIN.key"
echo "Add configuration in nginx:"
echo "server {"
echo "    ..."
echo "    listen 443 ssl;"
echo "    ssl_certificate     /etc/nginx/ssl/$DOMAIN.crt;"
echo "    ssl_certificate_key /etc/nginx/ssl/$DOMAIN.key;"
echo "}"
```


### Nginx配置

```
server {
  listen 80;
  server_name blog.ely.me;
  rewrite ^/(.*)$ https://blog.ely.me/$1 permanent;
}

server {
  listen 443 ssl;
  server_name www.blog.ely.me;
  ssl_certificate     /etc/nginx/ssl/xx.xx.xx.crt;
  ssl_certificate_key /etc/nginx/ssl/xx.xx.xx.key;
  rewrite ^/(.*)$ https://blog.ely.me/$1 permanent;
}

server {
  listen 443 ssl;
  server_name blog.ely.me;

  location /nginx_status {
    allow 127.0.0.1;
    deny all;
    stub_status on;
  }

  root /var/www/blog.ely.me;

  access_log /var/log/nginx/blog.ely.me/access.log;
  error_log /var/log/nginx/blog.ely.me/error.log;

  ssl_certificate     /etc/nginx/ssl/xx.xx.xx.crt;
  ssl_certificate_key /etc/nginx/ssl/xx.xx.xx.key;


  if (-f $document_root/system/maintenance.html) {
    rewrite ^(.*)$ /system/maintenance.html break;
  }

  location ~ (/assets|/system|/avatar.png|/favicon.ico|/*.txt) {
    access_log        off;
    expires           14d;
    gzip_static on;
    add_header  Cache-Control public;
  }

  location / {
    if ($host != 'blog.ely.me') {
      rewrite ^/(.*)$ https://blog.ely.me/$1 permanent;
    }
    try_files $uri $uri/ /index.php?$args;
    index index.php;
    include hhvm.conf;
    proxy_redirect     off;
    proxy_set_header   Host $http_host;
    proxy_set_header   X-Forward-For $remote_addr;
    proxy_set_header   Host $host;
    proxy_set_header   X-Forwarded-Host $host;
    proxy_set_header   X-Forwarded-Server $host;
    proxy_set_header   X-Real-IP        $remote_addr;
    proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
    proxy_buffering    on;
    proxy_http_version 1.1;
    proxy_set_header   Upgrade $http_upgrade;
    proxy_set_header   Connection "Upgrade";
    proxy_set_header   X-Forwarded-Proto https;
    gzip on;
  }
}
```


# 相关执行命令
```
service nginx restart|start|stop
service hhvm restart|start|stop
```

本文参考自[链接](https://lanmaowz.com/install-wordpress/),并重新整理
