---
layout:     post
title:      "macOS & iOS 系统优化设置"
date:       2019-09-13T02:16:45+08:00
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - macOS
    - iOS
---

> 本文主要记录在使用macOS和iOS过程中的一些自定义设置


# macOS
## 设置
#### [Mac关闭iTunes自动备份](https://blog.csdn.net/u013943420/article/details/81985192)
完全退出iTunes后，在终端运行如下命令

```
defaults write com.apple.iTunes DeviceBackupsDisabled -bool YES
```

重新打开自动备份

```
defaults delete com.apple.iTunes DeviceBackupsDisabled
```

# iOS
#### 引导式访问