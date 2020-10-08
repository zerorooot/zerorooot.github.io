---
title: 使用ProxyChains-NG翻墙
tags:
  - 翻墙
date: 2020-10-08 21:27:05
description: ProxyChains is a UNIX program, that hooks network-related libc functions in DYNAMICALLY LINKED programs via a preloaded DLL (dlsym(), LD_PRELOAD) and redirects the connections through SOCKS4a/5 or HTTP proxies. It supports TCP only (no UDP/ICMP etc).
---
proxychains通过hook系统相关代码，从而重定向连接到自定义的SOCKS4a/5 或 HTTP代理，从而实现翻墙的功能

# 安装

```
sudo apt install proxychains4
```

# 配置

proxychains-ng默认配置文件名为`/etc/proxychains4.conf`。

- 删除最后一行
- 添加 sock5 127.0.0.1 1080

# 使用

```
proxychains4 curl www.google.com
```

注：此程序只能使**某个**软件翻墙！！！
