---
layout:     post
title:      "Ubuntu 系统初始化"
subtitle:   "Ubuntu 系统初始化"
date:       2014-06-02
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - linux
    - ubuntu
---

## 系统初始化
#### Terminal Tab键忽略大小写自动补全
系统配置文件为/etc/inputrc
个人配置文件为~/.inputrc

修改个人配置文件后, 重启终端:
```
echo "set completion-ignore-case on" >> ~/.inputrc
```


### 1. 更新apt源
	$ apt-get update

### 2. 安装add-apt-repository
	$ apt-get install software-properties-common python-software-properties
    # 可能会提示安装失败,提示执行apt-get update:
    
    apt-get update --fix-missing
    apt-get install python-software-properties


### 3. 安装zip unzip
	$ apt-get install zip unzip

### 4. 设置系统语言
	$ vi /var/lib/locales/supported.d/local
	  zh_CN.UTF-8 UTF-8
	$ locale-gen

######  配置 ssh 免登陆访问
	$ vi .ssh/authorized_keys
	ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDVR3kIcLZHBntZgMkpiLyl11NOglWUc95UvjWfiPmjFFMJbNAQtaXNqdNKj4R4gC3heVyvEWmaYoVxhzwiwNV2SWdRUUQzviA5MmxgLkoNTXldK50j3b/zK0mxnRl3PMiBl21XKzz2psu4t4zkW5CM0F3aRminwRANdUryB5c7K5jxVLidvdcokZb+eTViCe+wUmwHI2xwjjzTSnJ8zMFrPJuVe9+0ZFQX8/VW1rtbv8TK4+GbjrAnolSp1he7Ok6bI5B8aIu5Lb9U17ZU9xNu4RuTH3/sr8v9XTnCqvYD69JXHxAc7RGO4seLQIMzthxTxmGtTlQBGUr9/sop/gsx ely.xiao@qq.com

### 5. 安装 Git
	$ add-apt-repository ppa:git-core/ppa
	$ apt-get update
	$ apt-get install git

### 6. zsh
```
# 安装zsh
$ apt-get install zsh

# 安装配置 oh-my-zsh
$ git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
$ cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
$ chsh -s /bin/zsh

# 安装配置 oh-my-zsh 的插件 - autojump
$ git clone git://github.com/joelthelion/autojump.git && cd autojump && ./install.py

$ vi ~/.zshrc
    [[ -s /root/.autojump/etc/profile.d/autojump.sh ]] && source /root/.autojump/etc/profile.d/autojump.sh
    autoload -U compinit && compinit -u

$ source ~/.zshrc

# 配置zsh主题
$ 参见 robbyrussell.zsh-theme.server
$ vi ~/.oh-my-zsh/themes/robbyrussell.zsh-theme
```

### 9.nginx
```
# 安装
apt-add-repository ppa:nginx/stable
apt-get update
apt-get install nginx

// 卸载
# apt-get remove nginx  // 删除nginx，保留配置文件
# rm -rf /etc/nginx     // 手动删除配置文件
# apt-get purge nginx   // 删除nginx，连带配置文件

# 配置nginx:
$ vi /etc/nginx/nginx.conf
include /etc/nginx/sites-enabled/*; (将这行注释掉)

$ vi /etc/nginx/conf.d/ely.conf
server {
    listen 80;
    server_name www.ely.me;

    location / {
        proxy_pass http://127.0.0.1:8080/;
    }
}

$ service nginx restart
```
### 11. java
	$ add-apt-repository ppa:webupd8team/java
	$ apt-get update
	$ apt-get install oracle-java7-installer
	$ apt-get install oracle-java8-installer

### 12. nodejs
	$ add-apt-repository ppa:chris-lea/node.js && apt-get update && apt-get install nodejs
	
### 安装 PHP
	$ apt-get install php5-cgi php5-fpm php5-mysql
	
	$ vi /etc/php5/fpm/pool.d/www.conf 
		# listen = /var/run/php5-fpm.sock
        listen = 127.0.0.1:9000


### 测试Nginx & PHP 搭建是否正常
	$ mkdir -p /var/www/wordpress && vi /var/www/wordpress/index.php
	  <? phpinfo();?>

	service php5-fpm restart
	service nginx restart
	
### nginx.conf for wordpress
~~~ shell
server {
    listen 80;

    root /var/www/wordpress;
    index index.html index.htm index.php;
    # Make site accessible from http://localhost/
    server_name blog.ely.me;

    location / {
            # First attempt to serve request as file, then
            # as directory, then fall back to index.html
            try_files $uri $uri/ /index.html;
            # Uncomment to enable naxsi on this location
            # include /etc/nginx/naxsi.rules
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

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    location ~ \.php$ {
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
    #       # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
    #
    #       # With php5-cgi alone:
            fastcgi_pass 127.0.0.1:9000;
    #       # With php5-fpm:
    #       fastcgi_pass unix:/var/run/php5-fpm.sock;
            fastcgi_index index.php;
            include fastcgi_params;
    }
}
~~~

### 16. 配置环境变量
	$ vi ~/.zshrc
    ALL_IP=$(ifconfig | grep inet | grep -v 127.0.0.1 | grep -v inet6 | awk '{print $2}' | tr -d "addr:")
	cat logo
    echo "公网：$ALL_IP                                                   内网：xxx.xxx.xxx.xxx "
    echo "-------------------------------------------------------------------------------------------"

    alias -s java=vi
    alias -s go=vi
    alias -s html=vi
    alias -s js=vi
    alias -s css=vi
    alias -s gz='tar -xzvf'
    alias -s tgz='tar -xzvf'
    alias -s bz2='tar -xjvf'
    alias -s zip='unzip'
    alias -s rar='unrar x'

    # Java
    export JAVA_HOME=/usr/lib/jvm/java-7-oracle
    export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib
    export PATH=$JAVA_HOME/bin:$PATH

    # Gradle
    export GRADLE_HOME=/root/sdk/gradle-2.3
    export PATH=$GRADLE_HOME/bin:$PATH

    # Tomcat
    export TOMCAT_HOME=/root/sdk/apache-tomcat-6.0.43


### 19. 服务器安全设置
##### 19.1 SSH 默认端口更改(修改完端口后，客户端需要配置ssh的config文件，指明端口)
    $ vi /etc/ssh/sshd_config
    Port XXXX (将端口修改为自己想要的)

    $ service ssh restart

##### 19.2 详情[http://coralzd.blog.51cto.com/](http://coralzd.blog.51cto.com/)
```
#!/bin/sh
# desc: setup linux system security
# author:coralzd
# powered by www.freebsdsystem.org
# version 0.1.2 written by 2011.05.03
#account setup

passwd -l xfs
passwd -l news
passwd -l nscd
passwd -l dbus
passwd -l vcsa
passwd -l games
passwd -l nobody
passwd -l avahi
passwd -l haldaemon
passwd -l gopher
passwd -l ftp
passwd -l mailnull
passwd -l pcap
passwd -l mail
passwd -l shutdown
passwd -l halt
passwd -l uucp
passwd -l operator
passwd -l sync
passwd -l adm
passwd -l lp

# chattr /etc/passwd /etc/shadow
chattr +i /etc/passwd
chattr +i /etc/shadow
chattr +i /etc/group
chattr +i /etc/gshadow
# add continue input failure 3 ,passwd unlock time 5 minite
sed -i 's#auth        required      pam_env.so#auth        required      pam_env.so\nauth       required       pam_tally.so  onerr=fail deny=3 unlock_time=300\nauth           required     /lib/security/$ISA/pam_tally.so onerr=fail deny=3 unlock_time=300#' /etc/pam.d/system-auth
# system timeout 5 minite auto logout
echo "TMOUT=300" >>/etc/profile

# will system save history command list to 10
sed -i "s/HISTSIZE=1000/HISTSIZE=10/" /etc/profile

# enable /etc/profile go!
source /etc/profile

# add syncookie enable /etc/sysctl.conf
echo "net.ipv4.tcp_syncookies=1" >> /etc/sysctl.conf

sysctl -p # exec sysctl.conf enable
# optimizer sshd_config

sed -i "s/#MaxAuthTries 6/MaxAuthTries 6/" /etc/ssh/sshd_config
sed -i  "s/#UseDNS yes/UseDNS no/" /etc/ssh/sshd_config

# limit chmod important commands
chmod 700 /bin/ping
chmod 700 /usr/bin/finger
chmod 700 /usr/bin/who
chmod 700 /usr/bin/w
chmod 700 /usr/bin/locate
chmod 700 /usr/bin/whereis
chmod 700 /sbin/ifconfig
chmod 700 /usr/bin/pico
chmod 700 /bin/vi
chmod 700 /usr/bin/which
chmod 700 /usr/bin/gcc
chmod 700 /usr/bin/make
chmod 700 /bin/rpm

# history security

chattr +a /root/.bash_history
chattr +i /root/.bash_history

# write important command md5
    cat > list << "EOF" &&
    /bin/ping
    /bin/finger
    /usr/bin/who
    /usr/bin/w
    /usr/bin/locate
    /usr/bin/whereis
    /sbin/ifconfig
    /bin/pico
    /bin/vi
    /usr/bin/vim
    /usr/bin/which
    /usr/bin/gcc
    /usr/bin/make
    /bin/rpm
    EOF

    for i in `cat list`
    do
       if [ ! -x $i ];then
       echo "$i not found,no md5sum!"
      else
       md5sum $i >> /var/log/`hostname`.log
      fi
    done
    rm -f list

```


## Error

1. 如果提示 `The program ‘add-apt-repository’ is currently not installed. You can install it by typing: apt-get install python-software-properties` 执行: 
```
apt-get install software-properties-common python-software-properties
```
