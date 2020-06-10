---
layout:     post
title:      "Ely At CentOS"
subtitle:   "Ely At CentOS"
date:       2016-11-11

banner_img: /img/post_banner_common.jpg

tags:
    - linux
---

### 1. 更新yum源
	$ yum update

### 2. 安装zip unzip
	$ yum install zip unzip

### 3. 设置系统语言
	$ vi /var/lib/locales/supported.d/local
	  zh_CN.UTF-8 UTF-8
	$ locale-gen

### 4. 配置 ssh 免登陆访问
	$ vi .ssh/authorized_keys
	ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDVR3kIcLZHBntZgMkpiLyl11NOglWUc95UvjWfiPmjFFMJbNAQtaXNqdNKj4R4gC3heVyvEWmaYoVxhzwiwNV2SWdRUUQzviA5MmxgLkoNTXldK50j3b/zK0mxnRl3PMiBl21XKzz2psu4t4zkW5CM0F3aRminwRANdUryB5c7K5jxVLidvdcokZb+eTViCe+wUmwHI2xwjjzTSnJ8zMFrPJuVe9+0ZFQX8/VW1rtbv8TK4+GbjrAnolSp1he7Ok6bI5B8aIu5Lb9U17ZU9xNu4RuTH3/sr8v9XTnCqvYD69JXHxAc7RGO4seLQIMzthxTxmGtTlQBGUr9/sop/gsx ely.xiao@qq.com

### 5. 安装 Git
	$ yum install git -y

### 6.安装 zsh
	$ yum install zsh -y

### 7.安装配置 oh-my-zsh
	$ git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
	$ cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
	$ chsh -s /bin/zsh

### 8.配置主题
    $ 参见 robbyrussell.zsh-theme.server
    $ vi ~/.oh-my-zsh/themes/robbyrussell.zsh-theme

### 9.安装nginx
    $ vi /etc/yum.repos.d/nginx.repo
    [nginx]
    name=nginx repo
    baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
    gpgcheck=0
    enabled=1
    
    $ yum install nginx

### 10. 安装mysql
```
# 前往mysql官网下载mysql的repo rpm安装文件 (包含了mysql不同版本，默认启用了5.7，安装其它版本需要修改repo文件以启用)
wget wget http://repo.mysql.com//mysql57-community-release-el7-9.noarch.rpm
# 安装repo
yum -y install mysql57-community-release-el7-9.noarch.rpm
提示:
vi /etc/yum.repos.d/mysql-community.repo 去启用不同的repo以安装不同的mysql版本
# 安装mysql-server
yum -y install mysql-community-server
# 安装mysql-client
yum -y install mysql-community-client

# 初始化DB
mysqld --initialize (mysql 5.7)
mysql_install_db --user=root (mysql 5.5)

# 查看root用户的初始化密码
cat /var/log/mysqld.log | grep "A temporary password"

# 修改DB文件的访问权限
chown -R mysql:mysql /var/lib/mysql

# 启动mysql
service mysqld start (CentOS 6)
systemctl start mysqld.service (CentOS 7)
```

### 11. 安装vsftpd
```
# 安装
$ yum install ftp
$ yum install vsftpd
$ service vsftpd restart

# 创建用户
$ groupadd ftp
$ mkdir /home/ftp
$ useradd -d /home/ftp -g ftp -s /sbin/nologin ftp
$ passwd ftp

# vsftp配置
    /etc/vsftpd/vsftpd.conf
    
# 测试新创建的用户是否能登陆ftp
$ ftp
ftp> open localhost
Connected to localhost.
220 (vsFTPd 2.3.5)
Name (localhost:root): ftp
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
```
### 12. 安装Java
	$ yum search java | grep jdk
	$ yum install java-1.8.0-openjdk-devel.x86_64



### 19. 服务器安全设置
##### 19.1 SSH 默认端口更改(修改完端口后，客户端需要配置ssh的config文件，指明端口)
```
$ vi /etc/ssh/sshd_config
Port XXXX (将端口修改为自己想要的)

$ service ssh restart
```

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

