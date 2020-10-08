---
title: 使用Privoxy翻墙
tags:
  - privoxy
  - 翻墙
date: 2020-10-08 21:34:53
description:当我们安装了ssr后，直接使用是不行的，因为ssr支持的是SOCKS5，而我们要用的是http代理，所以需要privoxy转换
---
sr后](https://zerorooot.github.io/2020/10/08/Debian-Ubuntu-%E5%AE%89%E8%A3%85-ssr/)，直接使用是不行的，因为ssr支持的是SOCKS5，而我们要用的是http代理，所以需要privoxy转换

### 安装

```bash
sudo apt install privoxy
```



安装失败尝试把

```
/lib/systemd/system/privoxy.service
中的
ExecStart=/usr/sbin/privoxy  --pidfile $PIDFILE --user $OWNER $CONFIGFILE
换成
ExecStart=/usr/sbin/privoxy  --no-daemon --pidfile $PIDFILE --user $OWNER $CONFIGFILE
```

### 配置

```bash
sudo vim /etc/privoxy/config
```

找到 `listen-address` 确保有这行代码 `listen-address 0.0.0.0:8118`

找到 `forward-socks5` 确保有这行代码并且打开注释(没有自己加)` forward-socks5 / 127.0.0.1:1080 .`



### 启动

```bash
sudo service privoxy start
```



### 配置环境变量，让终端也能走代理

```bash
# 全局环境变量
sudo vim /etc/profile
# ======================
# 文件末尾处都添加上以下内容
export http_proxy="http://127.0.0.1:8118"
export https_proxy="https://127.0.0.1:8118"
```



### 验证一下

```bash
curl www.google.com
```

发现可以上了，是不是很开心，再来试一下：

```bash
# 搜狐的这个接口能够返回你的IP地址
curl  -L tool.lu/ip
```

发现我们目前在终端任何HTTP连接都走了代理了，这不符合我们的预期，我们希望的让代理走pac模式：需要的连接才代理，不需要的就不用代理。

好了，我们来看看`Privoxy`如何使用pac模式，首先需要一个符合`Privoxy`的pac规则的文件，可以使用[PrivoxyGfwWhiteList](https://github.com/zerorooot/PrivoxyGfwWhiteList)来生成。

首先把`/etc/privoxy/config`中的`forward-socks5 / 127.0.0.1:1080 .`这一行注释了

### 安装 PrivoxyGfwWhiteList

```bash
git clone https://github.com/zerorooot/PrivoxyGfwWhiteList.git

cd PrivoxyGfwWhiteList

sudo bash -c 'echo "actionsfile whitelist.action" >> /etc/privoxy/config'

sudo java whiteList.java 127.0.0.1:1080
```



如果以后某个pac文件之外的网站也想走代理的话，那么仅需要把域名添加到template.txt文件里面就可以了，



### 重启Privoxy，测试代理是否走了pac模式

```bash
curl www.google.com
```

OK，没问题，google还是可以访问的；

```bash
curl "http://pv.sohu.com/cityjson?ie=utf-8"
```

这个也OK，返回的是我们自己宽带的IP，也没问题。



### 过滤本机地址

```bash
sudo vim /etc/privoxy/config
```

在最后添加

```
forward         192.168.*.*/     .
forward           127.*.*.*/     .
```


