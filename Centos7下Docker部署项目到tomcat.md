---
title: Centos7下Docker部署项目到tomcat
date: 2018-05-23 15:29:24
tags:
---

### Centos7下Docker部署项目到tomcat

**安装Docker**

* 设置yum安装源
``` bash
yum install -y epel-release
```
* 安装docker
``` bash
yum install docker-io
```
* 加入开机启动配置
``` bash
chkconfig docker on
```
* 启动docker服务
``` bash
service docker start
```


**配置docker镜像加速器**

用docker默认的镜像拉取docker镜像的速度太慢了，可以配置阿里云的镜像加速器，这样从国内的服务器拉取，速度刚刚的。可以登录阿里云，到[容器Hub服务控制台](https://cr.console.aliyun.com/?spm=a2c4e.11153940.blogcont29941.10.5c6e69d6B6SGKI)查看镜像加速地址，阿里云对每个用提供不同的加速地址。
* 可参考文章：[Docker 镜像加速器](https://yq.aliyun.com/articles/29941?spm=5176.10695662.1996646101.searchclickresult.696064a352OVFc)

如图：
![阿里云镜像](images/docker7.png)
![加速镜像](images/docker1.png)

* 配置加速镜像地址

``` bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://qylibup5.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

**docker安装Tomcat容器镜像**
* 查找tomcat容器镜像
``` bash
docker search tomcat
```
如图:
![](images/docker2.png)

* 下载start最多的tomcat镜像
``` bash
docker pull  docker.io/tomcat
```
如图：
![](images/docker3.png)

* 查看已经下载到本地的镜像
``` bash
docker images
```
如图：
![](images/docker4.png)

* 启动tomcat容器
``` bash
docker run -p 8080:8080 docker.io/tomcat
```
若端口被占用，可以指定容器和主机的映射端口，前者是主机对外访问端口：后者是映射到容器内部的端口

如图：
![](images/docker5.png)

* 用浏览器访问主机对应地址和端口

![](images/docker8.png)
