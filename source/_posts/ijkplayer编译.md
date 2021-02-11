---
title: ijkplayer编译
tags:
  - ijkplayer
date: 2021-02-11 13:31:02
description: 使用GSYVideoPlayer播放mkv格式的视频时，发现有画面没有声音。去issues逛了一圈，原来是不支持。于是就有了这篇文章
---

```bash
Q:为什么要自己编译ijkplayer?
A:因为别人编译的，对于自己来说，可能部分格式不支持
```

# 下载以及配置NDK

去[Android NDK官网](https://developer.android.com/ndk/downloads/older_releases)，下载**`android-ndk-r13b`**的版本

```bash
unzip android-ndk-r13b-linux-x86_64.zip
vim ~/.bashrc
```

输入

```bash
export ANDROID_NDK=/home/ubuntu/Android/android-ndk-r13b
```

把**/home/ubuntu/Android/**替换成你ndk的实际目录

别忘了刷新bashrc

```bash
source ~/.bashrc
```

ndk就配置好了

# 下载、配置并编译ijkplayer

## 下载

```bash
git clone https://github.com/bilibili/ijkplayer.git
cd ijkplayer
```

## 配置ijkplayer

这步就是关键了，通过不同的配置来让我们编译的ijkplayer支持不同格式的视频播放

### 前置知识

> 视频文件有不同的格式，用不同的后缀表示：avi，rmvb，mp4，flv，mkv等等（当然也使用不同的图标）。在这里需要注意的是，这些格式代表的是**封装格式**。何为封装格式？就是把视频数据和音频数据打包成一个文件的规范。仅仅靠看文件的后缀，很难能看出具体使用了什么**视音频编码标准**。总的来说，不同的封装格式之间差距不大，各有优劣。相关内容可以看[这篇博客](https://blog.csdn.net/leixiaohua1020/article/details/18893769)，讲的很详细

主要封装格式一览

| 名称 | 推出机构           | 流媒体 | 支持的视频编码                 | 支持的音频编码                        | 目前使用领域   |
| ---- | ------------------ | ------ | ------------------------------ | ------------------------------------- | -------------- |
| AVI  | Microsoft Inc.     | 不支持 | 几乎所有格式                   | 几乎所有格式                          | BT下载影视     |
| MP4  | MPEG               | 支持   | MPEG-2, MPEG-4, H.264, H.263等 | AAC, MPEG-1 Layers I, II, III, AC-3等 | 互联网视频网站 |
| TS   | MPEG               | 支持   | MPEG-1, MPEG-2, MPEG-4, H.264  | MPEG-1 Layers I, II, III, AAC,        | IPTV，数字电视 |
| FLV  | Adobe Inc.         | 支持   | Sorenson, VP6, H.264           | MP3, ADPCM, Linear PCM, AAC等         | 互联网视频网站 |
| MKV  | CoreCodec Inc.     | 支持   | 几乎所有格式                   | 几乎所有格式                          | 互联网视频网站 |
| RMVB | Real Networks Inc. | 支持   | RealVideo 8, 9, 10             | AAC, Cook Codec, RealAudio Lossless   | BT下载影视     |

因此，如果想要播放mkv格式，有两种方法，一种是查看当前视频的音频编码，让我们自己编译的ijkplayer支持此音频编码。另一种就是让我们的ijkplayer支持所有的音频、视频编码。前者占用的空间更小，但还是不支持部分视频；后者占用的空间大，但支持全部的视频、音频编码。这两种方法我们都来试一试

### 第一种

```bash
wget "https://github.com/CarGuo/GSYVideoPlayer/raw/master/module-lite.sh" -o config/module.sh
```

使用相关的软件查看当前视频的音频编码，ffmpeg所支持的格式可以在这里找到，按照我们下载的module.sh，照葫芦画瓢即可

```bash
./extra/ffmpeg/configure -h
  --list-decoders          show all available decoders
  --list-encoders          show all available encoders
  --list-hwaccels          show all available hardware accelerators
  --list-demuxers          show all available demuxers
  --list-muxers            show all available muxers
  --list-parsers           show all available parsers
  --list-protocols         show all available protocols
  --list-bsfs              show all available bitstream filters
  --list-indevs            show all available input devices
  --list-outdevs           show all available output devices
  --list-filters           show all available filters
```

### 第二种

复制以下内容，到**config/module.sh**里

```bash
#! /usr/bin/env bash

#--------------------
# Standard options:
export COMMON_FF_CFG_FLAGS=

# Licensing options:
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-gpl"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-nonfree"

# Configuration options:
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-runtime-cpudetect"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-gray"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-swscale-alpha"

# Program options:
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-programs"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-ffmpeg"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-ffplay"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-ffprobe"

# Documentation options:
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-doc"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-htmlpages"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-manpages"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-podpages"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-txtpages"

# Component options:
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-avdevice"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-avcodec"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-avformat"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-avutil"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-swresample"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-swscale"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-postproc"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-avfilter"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-avresample"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-network"

# Hardware accelerators:
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-d3d11va"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-dxva2"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-vaapi"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-vdpau"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-videotoolbox"

# Individual component options:
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-encoders"

# ./configure --list-decoders
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-decoders"


export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-hwaccels"

# ./configure --list-muxers
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-muxers"

# ./configure --list-demuxers
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-demuxers"

# ./configure --list-parsers
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-parsers"

# ./configure --list-bsf
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-bsfs"

# ./configure --list-protocols
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-protocols"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-protocol=async"

export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-devices"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-filters"

# External library support:
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-iconv"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-audiotoolbox"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-videotoolbox"

export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-linux-perf"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-bzlib"
```

## 编译

文件配置好了，终于可以开始编译了

### 初始化openSSL和FFMPEG

这里会同步下载对应的代码，所以可能会比较耗时

```
./init-android-openssl.sh
./init-android.sh
```

### 编译

```bash
cd android/contrib
./compile-openssl.sh clean
./compile-ffmpeg.sh clean
./compile-openssl.sh all
./compile-ffmpeg.sh all
```

如果觉得编译所有版本太耗时间，可以使用下面这个命令来代替

```bash
 ./compile-ffmpeg.sh  armv5|armv7a|arm64|x86|x86_64
```

更多命令

```bash
./compile-ffmpeg.sh -h

Usage:
  compile-ffmpeg.sh armv5|armv7a|arm64|x86|x86_64
  compile-ffmpeg.sh all|all32
  compile-ffmpeg.sh all64
  compile-ffmpeg.sh clean
  compile-ffmpeg.sh check
```

### 生成对应的so

```bash
cd ..
./compile-ijk.sh all
```

会在当前位置的ijkplayer目录下生成我们所想要的包，把so复制到你项目的app/libs/下，然后在app/build.gradle中输入

```bash
plugins {
    id 'xxxxxxx'
}

android {
   //xxxxxxxxx
    sourceSets {
        main {
            jniLibs.srcDirs = ['libs']
        }
    }
  //xxxxxxx
}

dependencies {xxxxx}
```

来启用自己编译so文件

