---
title: raspberrypi如何pppoe拨号
tags:
  - raspberrypi
  - pppoe
date: 2020-10-08 20:24:06
description: sudo apt install pppoe pppoeconf -y && sudo pppoeconf
---
```bash
sudo apt install pppoe pppoeconf  -y
sudo pppoeconf
```

配置文件在
```bash
/etc/ppp/pap-secrets
```

您可以通过
```bash
sudo pon dsl-provider
```
来建立连接和通过
```bash
sudo poff dsl-provider
```
 来中断连接
