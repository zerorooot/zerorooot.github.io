---
title: linux系统如何添加swap分区
tags:
- swap
- linux
date: 2020-10-08 20:20:22
description: linux系统添加swap分区
---
安装dphys-swap服务 `sudo apt-get install dphys-swapfile -y` 

编辑/etc/dphys-swapfile

```
sudo vim /etc/dphys-swapfile
```

将 CONF_SWAPSIZE 的值修改成你想要的大小。 一般在内存小于2G的情况下，交换分区应为内存的2倍!(树莓派的话是2G最好)

然后，重新启动 dphys-swapfile 文件服务

```
sudo /etc/init.d/dphys-swapfile restart
```
