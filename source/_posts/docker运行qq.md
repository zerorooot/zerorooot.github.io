---
title: docker运行qq
tags:
  - docker
  - linux
date: 2020-12-22 23:28:44
description: 在wine中使用qq，发现它太流氓了。及时退出了程序，还是有几个exe在后台驻守，于是乎想到了使用docker来运行。
---

本文通过manjaro来演示此过程

# 安装docker

```
sudo pacman -S docker
```

# 配置镜像文件

通过配置镜像文件来加速下载docker镜像

```
sudo mkdir /etc/docker
sudo vim  /etc/docker/daemon.json
```

输入

```
{
  "registry-mirrors": ["https://jarp5cr2.mirror.aliyuncs.com"]
}
```

# 启动docker并安装QQ

```bash
sudo systemctl start docker
docker pull bestwu/qq:light-7.9.14308-2
```

# 首次启动时的配置

```bash
docker run -d --name qq --device /dev/snd  -v /tmp/.X11-unix:/tmp/.X11-unix  -v $HOME/TencentFiles:/TencentFiles -e DISPLAY=unix$DISPLAY   -e XMODIFIERS=@im=fcitx  -e QT_IM_MODULE=fcitx   -e GTK_IM_MODULE=fcitx  -e AUDIO_GID=`getent group audio | cut -d: -f3` -e VIDEO_GID=`getent group video | cut -d: -f3`   -e GID=`id -g`   -e UID=`id -u`   bestwu/qq:light-7.9.14308-2
```

具体各个参数的含义，还请看[这里](https://github.com/bestwu/docker-qq)

稍等一会，这时qq就应该启动了，正常登录即可

# 停止QQ

```
docker stop qq
```

# 第二次及其以后登录

```bash
docker ps -a
```

找到`bestwu/qq:light-7.9.14308-2`对应的`CONTAINER ID`，比如我的是 `c6978ae2c529`

然后运行

```bash
docker start c6978ae2c529
```

# 删除

```bash
docker stop qq
docker rm qq
docker rmi bestwu/qq:light-7.9.14308-2
```

