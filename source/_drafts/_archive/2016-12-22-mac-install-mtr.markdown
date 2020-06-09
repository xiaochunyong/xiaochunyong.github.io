---
layout:     post
title:      "mac install mtr"
subtitle:   "mac install mtr"
date:       2016-12-22
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - software
---

brew install mtr
### 使用brew安装mtr
```apple js
➜ ~ brew install mtr
==> Downloading https://homebrew.bintray.com/bottles/mtr-0.87.yosemite.bottle.ta
Already downloaded: /Library/Caches/Homebrew/mtr-0.87.yosemite.bottle.tar.gz
==> Pouring mtr-0.87.yosemite.bottle.tar.gz
==> Caveats
mtr requires root privileges so you will need to run `sudo mtr`.
You should be certain that you trust any software you grant root privileges.
==> Summary
🍺 /usr/local/Cellar/mtr/0.87: 8 files, 148K
➜ ~ sudo mtr google.com
sudo: mtr: command not found
➜ ~ mtr google.com
zsh: command not found: mtr
```

### 解决方法
sudo chown root:wheel /usr/local/Cellar/mtr/0.87/sbin/mtr
sudo chmod u+s /usr/local/Cellar/mtr/0.87/sbin/mtr

alias mtr='/usr/local/Cellar/mtr/0.87/sbin/mtr'
