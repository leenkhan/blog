
---
title: 用cloud9(c9)搭建自己的web IDE开发环境
date: 2018-06-08 22:45:24
tags:
---


**闲话**：因为嫌弃MBP15太重，尝试周末带ipad和折叠键盘出去，看书学习时偶尔需要敲一下书上的代码跑一下，于是仔细对比了coding.net的Cloud Studio和亚马逊的cloud9(c9)两款web IDE，先说一下我的体验，首先coding的Cloud Studio只有一个月的免费体验时间，开通的时部署到腾讯云的建站主机上，到期需要续费才能用，价格228块最近coding和腾讯云合作做活动，活动价128块。cloud9可以免费部署到亚马逊的EC2云服务器，1cpu1G内存的是免费的，但是开通aws账号需要信用卡,没信用卡的可以散了(话说这年代为什么还有人木有信用卡)。

### 安装cloud9(C9) web IDE的两种方式:
#### 方式一：在aws上安装

这种方式安装是最简单的，而且是全功能的c9版本，而且可以免费使用aws云服务器，但是aws在国内没有数据中心，速度有点慢，如果你不在意，可以用这种方式。
* **第一步** 注册aws账号，绑定信用卡
![注册界面](http://bulldog.qiniudn.com/aws_signup.png)

* **第二步:部署cloud9(c9)** 打开管理面板部署cloud9
![部署cloud9](http://bulldog.qiniudn.com/cloud9open.png)

![step1](http://bulldog.qiniudn.com/c9_create_step1.png)

![step2](http://bulldog.qiniudn.com/c9_create_step2.png)
记得选1cup1Gbmemory，这个是免费的，然后下一步，最好就弄完了。剩下的就是打开浏览器折腾吧。

加载画面:
![加载画面](http://bulldog.qiniudn.com/c9ide_loding1.png)

cloud9欢迎界面：
![cloud9欢迎界面](http://bulldog.qiniudn.com/c9welcome.png)

#### 方式二：自主云服务器安装

这种安装方式的好处是你可以有更多的自主权，可以在IDE界面里的consle可以做更多事情，而且用国内的云服务器的话速度应该能得到保障，缺点是cloud9的开源版本功能没有亚马逊上的cloud9功能全面，而且自己提供云服务器一般是要花银子的。不过这里要强调一下，用第一种方式部署cloud9时除了选择亚马逊自己EC2主机，也可选择用自己提供的云服务器，不过我没尝试过。

### **第一步（其实是N步）：** 
* 打开[c9 github页面链接](https://github.com/c9/core),按照安装文档进行安装配置
* 执行安装命令
![安装文档图](http://bulldog.qiniudn.com/c9_install_cmd.png)
**安装**
``` bash
git clone https://github.com/c9/core.git c9sdk
cd c9sdk
scripts/install-sdk.sh
```
* 启动服务
``` bash
node server.js
```

* 启动项说明
``` 
--settings       Settings file to use 以配置文件
--help           Show command line options.打印帮助
-t               Start in test mode 测试模式
-k               Kill tmux server in test mode
-b               Start the bridge server - to receive commands from the cli  [default: false] 
-w               Workspace directory 指定工作空间目录
--port           Port 指定访问端口
--debug          Turn debugging on 开启调试模式
--listen         IP address of the server 指定服务器IP
--readonly       Run in read only mode 以只读模式运行
--packed         Whether to use the packed version.
--auth           Basic Auth username:password  指定basic auth认证登陆的用户名和密码,格式: 用户名:密码
--collab         Whether to enable collab.
--no-cache       Don't use the cached version of CSS
```

指定自己的工作空间，并以Basic Auth认证方式启动
``` bash
ubuntu@VM-4-197-ubuntu:~$ ls
c9sdk  download  workspace
ubuntu@VM-4-197-ubuntu:~$ node c9sdk/server.js -w ~/workspace --auth abc:123456
```
说明一下c9启动后侦听的是127.0.0.1的ip，我设置--listen为外网ip好像无效，最后是用安装nginx反向代理来实现外网80端口访问的。

![c9登陆](http://bulldog.qiniudn.com/c9_basic_auth.png)

剩下的事情就是代码写的怎么样，和linux命令用的熟不熟的问题了
