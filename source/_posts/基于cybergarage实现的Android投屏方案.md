---
title: 基于cybergarage实现的Android投屏方案
tags:
  - android
  - DLAN
  - 投屏
date: 2021-02-17 17:06:24
description: 网上关于Android DLAN投屏的方案要么没有，要么写的不详细，让人没有看下去的欲望。所以我基于CyberGarage4Android这个项目，写了一篇简单的demo，帮助有需求的人快速上手开发。
---

# 准备工作

## 导包

编辑项目**根目录**的build.gradle，添加如下字段

```bash
allprojects {
    repositories {
        google()
        jcenter()
        maven { url "http://repo.cybergarage.org:8080/maven/repo/" }
    }
}
```

然后导包

```bash
implementation 'org.cybergarage.upnp:cybergarage-upnp-core:2.1.1'
```

## 配置权限

打开**AndroidManifest.xml**，输入

```bash
<uses-permission android:name="android.permission.INTERNET" />
```

## 导入控制文件

下载[控制cybergarage](https://github.com/zerorooot/CyberGarage4Android/tree/master/dlna-lib/src/main/java/com/android/dlna)的三份文件，导入你的项目中，这三份文件是我们实现DLAN的核心

# 开始

## 获取可用的设备

新建一个用于监听的方法和用于存储返回数据的map

```java
private HashMap<String, Device> hashMap = new HashMap<>();

private final DLNADeviceManager.MediaRenderDeviceChangeListener mListener = new DLNADeviceManager.MediaRenderDeviceChangeListener() {
        @Override
        public void onStarted() {}

        @Override
        public void onDeviceListChanged(List<Device> list) {
            list.forEach(s -> {
                if (hashMap.get(s.getFriendlyName()) == null) {
                    hashMap.put(s.getFriendlyName(), s);
                }
            });
        }

        @Override
        public void onFinished() {}
    };
```

启用监听

```java
DLNADeviceManager.getInstance().startDiscovery(mListener);
```

当搜索到局域网里符合条件的设备后，`MediaRenderDeviceChangeListener`会把`Device`放入我们的`hashMap`中。

完成后，别忘了关闭搜索

```java
DLNADeviceManager.getInstance().stopDiscovery();
```

## 控制设备

创建控制方法

```java
IController mController = new MultiPointController();
```

通过mController，我们就可以控制DLAN的播放、暂停、声音、进度等

比如播放某视频

```java
String url = "http://video19.ifeng.com/video07/2013/11/11/281708-102-007-1138.mp4";
hashMap.forEach((k, v) -> {
       mController.play(v, url);
});
```

停止播放

```java
hashMap.forEach((k, v) -> {
      mController.stop(v);
});
```

等等

## 获取状态

通过`new IController.PlayerMonitor()`来获取，具体方法还请自己继承，这里为了方便仅展示了两个方法。

```java
mController.setPlayMonitor(new IController.PlayerMonitor() {
    .....
	@Override
    public void onVolumeChanged(int current) {}
    @Override
    public void onProgressUpdated(int currentTimeSeconds) {}
    ....
};
```

好了，Android的DLAN就是这么的简单~

# 感谢

感谢[CyberGarage4Android](https://github.com/zhangwenxue/CyberGarage4Android)，项目中的核心文件就是从这个项目中~~抄袭~~借鉴的