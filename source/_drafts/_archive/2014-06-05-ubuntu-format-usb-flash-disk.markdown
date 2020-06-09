---
layout:     post
title:      "Ubuntu 格式化 U盘"
subtitle:   "Ubuntu 格式化 U盘"
date:       2014-06-05

banner_img: /img/post_banner_common.jpg

tags:
    - software
---

查看设备,找到U盘对应的,假设为 /dev/sdc4
sudo fdisk -l

要格式化U盘 必须先卸载, 再格式化

卸载：
sudo umount /media/ely/GSP1RMCULXF

格式化：
sudo mkfs.vfat /dev/sdc4

不同的格式对应不同的格式化命令：
mkfs mkfs.cramfs mkfs.ext3 mkfs.ext4dev mkfs.msdos mkfs.vfat
mkfs.bfs mkfs.ext2 mkfs.ext4 mkfs.minix mkfs.ntfs
