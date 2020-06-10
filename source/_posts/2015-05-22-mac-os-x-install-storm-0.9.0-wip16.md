---
layout:     post
title:      "Mac OS X 10.10 安装 Storm-0.9.0-wip16"
subtitle:   "Mac OS X 10.10 安装 Storm-0.9.0-wip16"
date:       2015-05-22
banner_img: /img/post_banner_common.jpg
categories:
  - macOS
tags:
    - 工作随笔
    - storm
    - zeromq
    - jqzm
---

### Requirements
```
// install brew
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

// install build-tools
brew install pkg-config 
brew install automake
brew install libtool
``` 


### 1. 安装zookeeper

### 2. install storm
```
download storm-0.9.0-wip16.zip
unzip storm-0.9.0-wip16.zip
cd storm-0.9.0-wip16
vi conf/storm.yaml
```


##### 2.1 install zeromq
```   
$ cd /usr/local
$ git checkout 381c97f Library/Formula/zeromq.rb
$ brew install zeromq
```


##### 2.2 install jzmq
```
$ git clone https://github.com/nathanmarz/jzmq.git
$ cd jzmq
$ ./autogen.sh
autoreconf: Entering directory `.'
autoreconf: configure.ac: not using Gettext
autoreconf: running: aclocal --force 
autoreconf: configure.ac: tracing
autoreconf: configure.ac: not using Libtool
autoreconf: running: /usr/local/Cellar/autoconf/2.69/bin/autoconf --force
configure.ac:14: error: possibly undefined macro: AC_PROG_LIBTOOL
      If this token and others are legitimate, please use m4_pattern_allow.
      See the Autoconf documentation.
autoreconf: /usr/local/Cellar/autoconf/2.69/bin/autoconf failed with exit status: 1

出现这种情况的时候，请检查/usr/local/lib 和 /usr/local/include 文件夹的权限(这两个目录是brew在使用)


$ ./configure
$ make
...
make[1]: *** No rule to make target classdist_noinst.stamp', needed byorg/zeromq/ZMQ.class'.  Stop.
make: *** [all-recursive] Error 1
    
解决方法:
touch src/classdist_noinst.stamp

$ make
...
make[1]: *** No rule to make target org/zeromq/ZMQException.class, needed byall'.  Stop.
make: *** [all-recursive] Error 1

解决方法:
cd src
javac -d . org/zeromq/*.java

$ cd ../
$ make
...
In file included from ZMQ.cpp:25:
In file included from ./util.hpp:23:
/Library/Java/JavaVirtualMachines/jdk1.7.0_71.jdk/Contents/Home/include/jni.h:45:10: fatal error: 'jni_md.h' file not found
#include "jni_md.h"
         ^
1 error generated.
make[2]: *** [libjzmq_la-ZMQ.lo] Error 1
make[1]: *** [all] Error 2
make: *** [all-recursive] Error 1

解决方法:
$ vi config.status
找到如下行
S["CPPFLAGS"]="-D_REENTRANT -D_THREAD_SAFE  -I/usr/local/Cellar/zeromq/2.1.9/include  -I/Library/Java/JavaVirtualMachines/jdk1.7.0_71.jdk/Contents/Home/include"
修改为
S["CPPFLAGS"]="-D_REENTRANT -D_THREAD_SAFE  -I/usr/local/Cellar/zeromq/2.1.9/include  -I/Library/Java/JavaVirtualMachines/jdk1.7.0_71.jdk/Contents/Home/include -I/System/Library/Frameworks/JavaVM.framework/Versions/Current/Headers/"
    

$ sudo make install
```


### 参考文章:
[http://storm.apache.org/documentation/Installing-native-dependencies.html](http://storm.apache.org/documentation/Installing-native-dependencies.html)
[http://nsdifficult.com/blog/20140325/storm/](http://nsdifficult.com/blog/20140325/storm/)
[http://xxthermidorxx.hatenablog.com/entry/2014/03/15/225926](http://xxthermidorxx.hatenablog.com/entry/2014/03/15/225926)
[https://github.com/oschrenk/zeromq-java-sample/blob/master/README.md](https://github.com/oschrenk/zeromq-java-sample/blob/master/README.md)
