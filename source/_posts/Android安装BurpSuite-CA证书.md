---
title: Android安装BurpSuite CA证书
tags:
  - BurpSuite
  - Android
date: 2021-01-15 10:42:42
description: openssl x509 -inform der -in mycert.der -out mycert.cer
---

# 获取BurpSuite CA证书

## 方法一

让浏览器开了127.0.0.1:8080代理，通过浏览器访问  http://burp 

## 方法二

打开BurpSuite，点击

proxy->options->import/export CA certificate->Certificate in DER format

选择导出的文件夹和文件名，点next 即可

# 将证书转成Android所能识别的

```bash
openssl x509 -inform der -in cacert.der -out ca.crt
cat ca.crt > $(openssl x509 -inform PEM -subject_hash_old -in ca.crt  | head -1).0
```

这时候会输出一个xxxx.0的文件，这个文件就是我们所需要的，我这里是9a5ba575.0

注：请把cacert.der改成你导出ca证书的文件名

# 导入Android

## 方法一

把输出的文件"xxxxx.0"移动到 /system/etc/security/cacerts/并给与644权限

## 方法二

### 安装证书

在手机上，打开设置->安全和锁屏->凭据存储->从存储设备中安装

安装上文生成的"ca.crt"证书

### 安装move certificates

在magisk中，搜索"move certificates"并安装，重启即可