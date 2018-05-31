---
title: Mac OS X中利用终端校验文件的MD5和SHA1值
date: 2018-05-17 13:06:24
tags:
---

本文转载自：[http://bzsy.iteye.com/blog/1851064](http://bzsy.iteye.com/blog/1851064)

一直使用HashMate（[App Store链接](https://itunes.apple.com/cn/app/hashmate/id555593874?mt=12)）计算MD5值，这是一款免费软件，把文件拖到它的状态栏图标上就可以得到文件MD5值，很方便。 

用终端计算MD5和SHA1值用以下命令： 

**计算MD5值**  ：md5 <文件路径> 

**计算SHA1值** ：shasum <文件路径> 
（指令和文件路径之间有空格）

**Examle**: 直接把文件拖到终端窗口可以自动添加文件路径。 
引用
* **如计算下载的windows 8镜像的SHA1值**： 

 shasum /Volumes/Macintosh\ HD/下载/cn_windows_8_pro_vl_x86_dvd_917720.iso

* **计算下载文件的SHA256值**:

shasum -a 256 /Users/smb/Downloads/ideaIU-181.5087.6.dmg






