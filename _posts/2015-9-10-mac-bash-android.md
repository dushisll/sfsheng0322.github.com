---
layout: post
author: 孙福生
title: Android手机录视频转Gif格式
cover: "zzz"
categories: 工具
tags: Util Mac
---
这篇博客主要为开发人员解决Android手机录视频转Gif格式难的痛点：<br/>
1、Mac系统下通过bash连接Android手机<br/>
2、通过adb shell命令获得录制的视频<br/>
3、通过Gifrecord软件将视频转化为Gif文件<br/>
4、补充一些adb的操作命令<br/>

### 以[《我的作品－Bingoworld》](http://sfsheng0322.github.io/2015/09/09/bingo.html)为例

<img src="/assets/bingo.gif" style="width: 50%;">

### 1、Mac系统下通过bash连接Android手机

1、Mac系统通过数据线连接Android手机

2、找到android手机的vendor ID：

* 终端执行CMD：  system_profiler SPUSBDataType
* 在列出的usb设备中找到自己的手机，copy下vendor ID

MI 4W: <br/>
Product ID: 0x0368<br/>
Vendor ID: 0x2717<br/>
Version: 2.32<br/>
Serial Number: 4fd453b9<br/>
Speed: Up to 480 Mb/sec<br/>
Manufacturer: Xiaomi<br/>

* 将vandor ID放到配置文件中：  ~/.android/adb_usb.ini
* 终端执行CMD ： vi  ~/.android/adb_usb.ini
* 将上面的vendor ID写到文件的最后面， :wq 保存退出

3、如果没有设置adb环境变量，设置一下：

* 终端执行CMD： vi ~/.bash_profile
* 在文件最后加上(path因电脑而异)： export PATH=/Users/sunfusheng/Android/Studio/sdk/platform-tools/:$PATH
* :wq 保存退出
* 终端执行CMD： source ~/.bash_profile
* adb devices 已经连接上adnroid手机了

### 2、通过adb shell命令获得录制的视频

* 录制命令

adb shell screenrecord /sdcard/test.avi
视频保存目录可以自己指定，如上面的/sdcard/test.avi，
命令执行后会一直录制180s，按下ctrl+c可以提前结束录制

* 设定视频分辨率

对于高分辨率的手机，录制的视频很大，我们分享又不需要这么大的
我们可以设置录制的视频分辨率
adb shell screenrecord - -size 848*480 /sdcard/test.avi

* 设定视频比特率

默认比特率是4M/s，为了分享方便，我们可以调低比特率为2M
adb shell screenrecord - -bit-rate 2000000 /sdcard/test.avi

* 获取视频文件

使用adb pull 即可把手机SD卡中视频获取到本地
adb pull /sdcard/test.avi .

### 3、通过Gifrecord软件将视频转化为Gif文件

转GIF文件
在Windows下有个不错的软件Free Video to GIF Converter可以把mp4转换成GIF。
转换时还可以删除不需要的帧，这点真得很不错。

Mac上可以使用Gifrocket进行转换，经测试后mp4格式会有问题，最好用avi格式的视频转Gif。

### 4、补充一些adb的操作命令

* adb dervices 显示当前启动的仿真器装置序号
* adb help 显示adb指令用法
* adb verson 显示adb版本
* adb install 安装APK应用程序组件
* adb push 上传文件或目录（adb push 文件所在PC的位置即文件名 目的位置）
* adb pull 下载文件或目录（adb pull 文件所在手机的位置即文件名 目的位置）
* adb shell 进入Android系统命令行模式
* adb logcat 监控仿真器运行记录
* adb bugreport 生成adb出错报告
* adb start－server启动adb服务器
* adb kill－server关闭adb服务器
* adb get－state取得adb服务器运行状态
* adb get－serialno获得仿真器运行序号



感谢CSDN博友的文章：[miaojun](http://blog.csdn.net/miaojunking/article/details/41053759)

感谢简书博友的文章：[阳春面](http://www.jianshu.com/p/9a1825e679b7?utm_campaign=haruki&utm_content=note&utm_medium=reader_share&utm_source=qq)
