---
title: 因为 accessExternalDTD 属性设置的限制导致不允许 'http' 访问
tags:
  - linux
date: 2021-01-02 21:41:18
description: 在manjaro上使用mybatis出现了此问题，但网上的资料都是jdk8的，而我用的是jdk11，所以就有了这篇文章.
---

# 前言

上网搜，大部分都是jdk8，在`%JAVA_HOEE%\jre\lib\` 目录下，新建一个文件jaxp.properties。但我TM用的是jdk11，用

```
sudo pacman -S jre11-openjdk-headless jre11-openjdk jdk11-openjdk openjdk11-doc openjdk11-src
```

安装的，故找不到这个文件。

# 解决

其实解决这个问题也很简单，jdk11把此配置文件换了个位置，放在了

```
/usr/lib/jvm/java-11-openjdk/conf/
```

这个目录下，所以我们只要

```bash
sudo bash -c 'echo "javax.xml.accessExternalSchema=all 
javax.xml.accessExternalDTD=all" >> /usr/lib/jvm/java-11-openjdk/conf/jaxp.properties'
```

即可