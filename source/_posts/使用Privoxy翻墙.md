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



### 再其他设备上翻墙

在配置好上述步骤后，且确保有这行代码 `listen-address 0.0.0.0:8118`后，就可以通过代理的方式，使其他设备翻墙

（要求此设备要和要翻墙的设备处于同一局域网下，即连同一个路由器

#### 查看本机地址

```bash
ifconfig
```

找到eth0或者wlan0下的 `inet`，此为你设备的ip地址

#### Android系统

1.点击“设置”，进入WiFi列表，长按要修改代理的WiFi。

2.弹出菜单中选择“修改网络”，或“连接到网络”。 

3.弹出窗口底部勾选“高级选项-代理设置：列表中选择手动”。 

4.服务器主机名填写刚刚看到的ip，服务器端口填写8118，保存后即可。

#### ios

1. 打开**设置** 进入**设置**，选择无线局域网
2. 进入wifi详细**设置** 找到已连接的wifi，点击最右侧的小图标
3. 配置**代理** 进入 配置**代理** 界面 选择“手动” 字段 填写方法 服务器 填入 刚刚看到的ip. 端口 8118

#### windows

1. 首先，按下键盘的win键，就是**windows** 图标。 然后再开始菜单中单击“**设置**”。
2. 然后，再出现的**设置**面板中，单击“网络和Internet”。
3. 单击“**代理**”。 你就会看到右侧有关**代理**的详细**设置**。
4. 下面，接下来看下手动代理，地址 填入 刚刚看到的ip. 端口 8118