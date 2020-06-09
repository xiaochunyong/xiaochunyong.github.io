---
layout:     post
title:      "Git 高级教程"
subtitle:   "Git 高级教程"
date:       2019-02-26
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - git
---

# 优雅的删除子模块
```
MOD_NAME=xxx
# 逆初始化模块，其中{MOD_NAME}为模块目录，执行后可发现模块目录被清空
git submodule deinit $MOD_NAME
# 删除.gitmodules中记录的模块信息（--cached选项清除.git/modules中的缓存）
git rm --cached $MOD_NAME
# 删除.gitmodules文件里的引用
vi .gitmodules
# 提交更改到代码库，可观察到'.gitmodules'内容发生变更
git commit -am "Remove a submodule." 
```


# 添加子模块
```
git submodule add git@xx.xx.xx:Creative/creative-model.git
```


# 附录
[Git分支管理实践](https://gitee.com/oschina/gitee_best_practices/blob/master/%E4%BB%A3%E7%A0%81%E7%AE%A1%E7%90%86/Git%E5%88%86%E6%94%AF%E7%AE%A1%E7%90%86%E5%AE%9E%E8%B7%B5.md#)
