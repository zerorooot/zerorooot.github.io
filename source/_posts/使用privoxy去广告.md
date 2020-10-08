---
title: 使用privoxy去广告
tags:
  - privoxy
  - ad
date: 2020-10-08 21:47:25
description: wget https://raw.githubusercontent.com/zerorooot/privoxy-adblock/master/privoxy-adblock.sh && sudo bash privoxy-adblock.sh && sudo bash -c 'echo "actionsfile easylist.script.action" >> /etc/privoxy/config' && sudo bash -c 'echo "filterfile easylist.script.filter" >> /etc/privoxy/config'
---
用完privoxy翻墙后，感觉只拿来翻墙有点浪费，而且去广告也算是一个毕竟刚需的内容，所以就有了这篇文章

# 使用privoxy-adblock去广告

```bash
wget https://raw.githubusercontent.com/zerorooot/privoxy-adblock/master/privoxy-adblock.sh
sudo bash privoxy-adblock.sh
sudo bash -c 'echo "actionsfile easylist.script.action" >> /etc/privoxy/config'
sudo bash -c 'echo "filterfile easylist.script.filter" >> /etc/privoxy/config'
```

# 自定义去广告

自定义去广告也使用privoxy-adblock去广告并不冲突，二者可以一起使用

在之前的privoxy翻墙文章里，我们使用了[PrivoxyGfwWhiteList](https://github.com/zerorooot/PrivoxyGfwWhiteList)来翻墙，这次，我们使用它来去广告

在 `template.txt`的上面，添加一行

```bash
banad = +forward-override{forward 127.0.0.1:444}
```

444端口是随意指定的，只要不让网站转发到真实服务器就好

然后，在 `template.txt`的最尾，添加

```bash
{banad}
```

在{banad}下面输入抓包得来的广告网址，如

```bash
{banad}
www.baidu.com
```

这样，百度就访问不了了

**注**：这种方法只能屏蔽http开头的网址，https开头的不行

**注**：这种方法只能屏蔽http开头的网址，https开头的不行

**注**：这种方法只能屏蔽http开头的网址，https开头的不行




