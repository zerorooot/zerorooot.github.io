---
title: 配置samba服务
tags:
  - samba
date: 2020-10-08 20:30:16
description: sudo apt install samba && sudo smbpasswd -a ubuntu
---
```bash
sudo apt install samba
```


添加用户(下面的ubuntu是我的用户名，之后会需要设置samba的密码)。 

```bash
sudo smbpasswd -a ubuntu
sudo vim /etc/samba/smb.conf
```

```
[ubuntu]
comment = share folder
browseable = yes
path = /home/ubuntu
create mask = 0700
directory mask = 0700
valid users = ubuntu
force user = ubuntu
force group = ubuntu
public = no
available = yes
writable = yes
```



测试你配置的smb.conf是否正确，用下面的命令

```bash
testparm
```

重启samba服务器。

```bash
sudo service smbd restart
```
