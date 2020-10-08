---
title: raspberrypi如何开热点
tags:
  - raspberry pi
date: 2020-10-08 20:22:27
description: 使用树莓派开热点
---
```
git clone https://github.com/oblique/create_ap 
cd create_ap 
sudo apt install util-linux procps hostapd iproute2 iw haveged dnsmasq iptables make -y
sudo make install 
sudo create_ap wlan0 eth0 rasberry 1234567890
```
