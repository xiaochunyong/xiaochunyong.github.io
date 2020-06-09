---
layout:     post
title:      "Build Elasticsearch Docs"
date:       2019-12-09T11:18:12+08:00
banner_img: /img/post_banner_common.jpg
tags:
    - elastic
---
> elastic官方文档, 浏览起来太慢了, 想构建一份本地的.


## 环境
macOS 10.15
docker


## Build
#### 1. 克隆elasticsearch (ES的文档在这里面)
```
# 如果速度太慢的话, 也可以选择直接下载zip包
git clone https://github.com/elastic/elasticsearch.git
```

#### 2. 克隆docs (构建工具)
``` 
git clone https://github.com/elastic/docs.git
```
克隆完成后, 可以看到目录下有一个 build_doc 和 doc_build_aliases.sh


###### doc_build_aliases.sh 脚本
这个脚本包含了elastic各种产品的文档构建命令, 有我们这次要构建的elasticsearch, 也有logstash, clouds, beats 等等. 我截取elasticsearch的命令:
```
# Elasticsearch
alias docbldesx='$GIT_HOME/build_docs --doc $GIT_HOME/elasticsearch/docs/reference/index.asciidoc --resource=$GIT_HOME/elasticsearch/x-pack/docs/ --chunk 1'
```

其中 $GIT_HOME/build_docs 指的是
```
docs/build_docs
```

而下述命令中的路径都是elasticsearch repo中的地址
```
--doc $GIT_HOME/elasticsearch/docs/reference/index.asciidoc
--resource=$GIT_HOME/elasticsearch/x-pack/docs/
```

我们可以将上述命令修改成成绝对路径  或者按命令中指定的路径存放.
alias docbldesx='/Users/Ely/Projects/Github/elastic-doc/build_docs --doc /Users/Ely/Projects/Github/elasticsearch/docs/reference/index.asciidoc --resource=/Users/Ely/Projects/Github/elasticsearch/x-pack/docs/ --chunk 1'


之后再执行修改后的命令

#### 开始构建
构建过程是在docker容器中执行的, 所以需要启动docker. 

```bash
$ docbldesx
```

构建完成后, 会输出到docs/html_docs的目录. 可能会缺失样式, 我是直接从官网抓包将缺失的css/js等资源文件下载下来.

然后启动一个http服务, 指定到html_docs目录.  就能在本地浏览了.