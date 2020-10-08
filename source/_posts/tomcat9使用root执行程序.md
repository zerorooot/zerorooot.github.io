---
title: tomcat9使用root执行程序
tags:
  - tomcat
  - linux
date: 2020-10-08 20:35:43
description: 现在可以tomcat可以执行root代码啦~( •̀ ω •́ )✧
---
##### 添加环境

sudo vim /lib/systemd/system/tomcat9.service

注释原来的

```
 AmbientCapabilities=CAP_NET_BIND_SERVICE
 NoNewPrivileges=true
```

添加

```
NoNewPrivileges=false
AmbientCapabilities=CAP_SETGID CAP_SETUID
SecureBits=keep-caps
```

##### 给tomcat用户设置密码

```
sudo passwd tomcat
```

##### 添加到sudo用户组

sudo vim /etc/sudoers

在root    ALL=(ALL:ALL) ALL后，添加

```
tomcat  ALL=(ALL:ALL) ALL
```

##### 使用

```
echo "你设置的tomcat密码" | sudo -S 要执行的命令
```

##### 权限

**更改tomcat9的UMASK**

/usr/share/tomcat9/bin目录下如果没有setenv.sh,添加此文件,内容如下:

```
UMASK=0022
```

使此文件可执行:

```
sudo chmod +x setenv.sh
```

重新启动tomcat9

```
sudo systemctl restart tomcat9
```
