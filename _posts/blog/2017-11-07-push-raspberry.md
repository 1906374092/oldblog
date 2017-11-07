layout:     post
title:      树莓派搭建rtmp推流服务器
category: blog
description: 在直播平台无限循环播放电影或音乐
---

**前言**
  
  觊觎树莓派好久了，终于一狠心，一跺脚，在某宝入手了一块树莓派3B。花了四百多块大洋，其实一块树莓派主板还不到两百大洋，但是某宝上的套件实在诱人，3.5寸lcd触摸屏，无线键鼠套装等，买回来才发现那些所谓的开发套件大多数都是鸡肋。不过总的来说还是很值得的，armv7架构，4核1G，运行基本的linux程序绰绰有余。对于不知道树莓派是干嘛用的小白，请自行百度，本教程适用有一定编程基础的童鞋～～～
  
--
**树莓派配置环境**

  树莓派没有硬盘，存储全靠一张tf内存卡，就是手机通用的那种。最好是闪迪class10的，class10读写速度快，闪迪兼容性好。
  拿到内存卡后，首先是格式化，操作流程不细说了，总之格式化成fat32文件系统就可以。然后去树莓派官网下载系统镜像，经过一番折腾感觉树莓派官方的raspbian系统没有ubuntu mate用着好，所以直接下载ubuntu mate版解压缩得到img镜像文件。然后打开命令行

```
df -h

```
查看当前文件系统的挂在卷，然后插上格式化好的tf卡，通常都是读卡器。再次输入 df -h命令，查看多出了哪个系统卷。多出的那个就是内存卡了。然后卸载卷：

```
sudo umount 你要卸载的卷

```
之后再运行一次 df -h查看是否卸载成功，然后写入镜像：

```
sudo dd bs=4m if=镜像绝对路径 of=内存卡所在的盘(eg:/dev/disk2)
```
需要几分钟时间，写好镜像后会显示写入大小。
写入成功后，运行命令推出tf卡：

```
sudo umountDisk 内存卡所在的盘(eg:/dev/disk2)
```
然后拔出内存卡，插到树莓派插槽上，通电开机，完美运行。
ubuntu mate默认用户名：ubuntu 密码：ubuntu。

OK，到这里我们就可以愉快的折腾树莓派啦😄。

**rtmp直播**

如果你的树莓派有显示屏，或者你直接使用ssh或vnc登录其实都可以，总之就是要进入unbuntu系统的终端，能使用命令行，就可以了。

1.安装ffmpeg推流工具

  使用apt-get install ffmpeg --fix-missing ，这样就会连同依赖包一起安装，安装完成后就可以直接使用了。
  在终端输入推流命令：
  
  ```
setsid ffmpeg -re -f concat -safe 0 -i playlist.txt -c copy -f flv 'rtmp://live-send.acg.tv/live/live_20572797_6228231?streamname=live_20572797_6228231&key=b084060931cdf8f37a629acedcdbec8e'
  ```
  setsid为程序后台运行的命令，引号内的rtmp链接为B站的直播码，也可以是斗鱼或其他平台都可以。playlist.txt文件是播放列表。里面的内容是这种格式的：
  
  ```
file '/media/UDisk/movie/无间道1.mp4'
file '/media/UDisk/movie/无间道2.mp4'
file '/media/UDisk/movie/无间道3.mp4'
  ```
  
 这样上面的命令运行成功会在终端内不断输出流信息。现在打开你的直播间。就能看到推流的内容了。
 下面放效果
 ![](http://ww3.sinaimg.cn/large/0060lm7Tly1fl9hwdppw3j31hc0u0npd.jpg)
 
 *大吉大利，大功告成！！！*