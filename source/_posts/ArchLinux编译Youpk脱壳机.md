---
title: ArchLinux编译Youpk脱壳机
tags:
  - arch
  - aosp编译
date: 2022-11-30 23:11:36
description: 本文记录了Youpk脱壳机在archlinux下的编译环境搭建和错误处理过程.
---

# 安装依赖

```bash
sudo pacman -S bc curl git gnupg gperf lib32-expat lib32-gcc-libs lib32-glib2 lib32-glibc lib32-libdbus lib32-libffi lib32-libpng lib32-ncurses lib32-pcre lib32-readline lib32-zlib libpng libxml2 libxslt ncurses perl-switch readline schedtool sdl squashfs-tools unzip wxgtk zip zlib repo python-virtualenv ccache base-devel
```

# 配置jdk8环境

```bash
sudo archlinux-java set java-8-openjdk
```

# 编译相关

具体教程可以查看`https://mirrors.ustc.edu.cn/help/aosp.html`

## 下载AOSP源码

```
mkdir aosp
cd aosp
#下载指定的android版本
repo init -u git://mirrors.ustc.edu.cn/aosp/platform/manifest -b android-7.1.2_r33 --depth 1 
#并发数不宜太高，否则会出现 503 错误
repo sync -j4
#repo sync --force-sync -j4
```

## 下载Youpk源码

```bash
#回到下载aosp的主目录
git clone https://github.com/Youlor/unpacker
cp -r -n unpacker/android-7.1.2_r33/* .
rm -rf unpacker/
```

## 配置编译环境

### 创建链接，防止出现so文件找不到

```bash
sudo ln /usr/lib64/libncursesw.so.6 /usr/lib/libncurses.so.5 
sudo ln  /usr/lib/libtinfo.so.6 /usr/lib/libtinfo.so.5
```

### 添加frameworks白名单

```bash
vim build/core/tasks/check_boot_jars/package_whitelist.txt
```

新起一行，添加

```
cn\.youlor
```

### 启用ccache缓存

```bash
#启用缓存
export USE_CCACHE=1
#缓存位置
export CCACHE_DIR=~/.ccache 
#缓存大小
ccache -M 50G
```

### 设置swap

```bash
#建一个 8G 的文件
sudo dd if=/dev/zero of=/swap bs=1M count=8192
#建立 Swap 的文件系统
sudo mkswap /swap
#启用 Swap 分区
sudo swapon /swap
#修改swappiness值为60,0表示最大限度使用物理内存,100表示积极的使用swap分区
sudo sysctl vm.swappiness=60
```

### 配置py2环境

```bash
virtualenv venv --python=python2.7
source venv/bin/activate
```

## 正式编译

```bash
#同步环境信息
source build/envsetup.sh
export LC_ALL=C
#选择 aosp_x86_64-eng
lunch
#正式开始编译
make -j$(nproc --all)
#等待编译结束后，别忘了清除缓存
#ccache -c
```



# 参考

https://wxy1343.xyz/2022/10/30/Youpk%E8%84%B1%E5%A3%B3%E6%9C%BA%E7%BC%96%E8%AF%91/
