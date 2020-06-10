---
layout:     post
title:      "Install Oracle Java On Ubuntu 12.04"
subtitle:   "Install Oracle Java On Ubuntu 12.04"
date:       2014-05-31

banner_img: /img/post_banner_common.jpg

tags:
    - software
---


    add-apt-repository ppa:webupd8team/java
    apt-get update
    apt-get install oracle-java7-installer

如果提示

The program ‘add-apt-repository’ is currently not installed. You can install it by typing:
apt-get install python-software-properties

执行

    apt-get install software-properties-common python-software-properties
