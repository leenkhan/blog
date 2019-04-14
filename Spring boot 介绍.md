---
title: Spring boot 介绍
date: 2019-01-14 21:55:24
tags:
---

# Spring boot 介绍 
## 迟来的Spring Boot学习序列（一）

* *spring boot是个啥?*
我们来看一下官方介绍：
>Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run".

大概意思就是：可以非常容易的用来创建基于spring体系的独立运行的应用。当然这里指等是一个微服务应用


* 怎么创建？show me the code
spring官方提供了一个网站可以通过自定义可选模块的方式创建。[Spring Initializr.](https://start.spring.io/)

![@创建spring boot项目](images/Spring-Initializr..png)

* 选择Grenrate project就生成一个初始化好的springboot项目的压缩包了，解压，导入开发IDE
![@IED](images/hello-project.png)

* 可以看一下项目，是一个普通的manve项目的结构：
* 注意一下resources文件夹,里面有两个文件目录
* src/resources/static，这个约定存放js、图片等静态资源
* src/resources/templates，这个约定存放html等模版页面

* pom.xml
```xml
<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
```
* HelloSpringbootApplication.java ，这是整个项目的启动入口类
```java
package io.github.leenkhan.hellospringboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

SpringBootApplication
public class HelloSpringbootApplication {

	public static void main(String[] args) {
		SpringApplication.run(HelloSpringbootApplication.class, args);
	}

}
```
* application.properties 这个是项目配置文件，数据库、及其他用到的中间件的配置一般在这里配置，目前内容是空白的
* 下面建我们的第一个微服务接口， 新建IndexController.java
```java
package io.github.leenkhan.hellospringboot.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class IndexController {

    @RequestMapping("/index")
    @ResponseBody
    public String index(){
        return "hello spring boot";
    }
}
```
启动HelloSpringbootApplication，访问http:http://localhost:8080/index
![@效果](images/hello-controller.png)

* 好的，你完成了一个简单等微服务应用了，你甚至可以打包你打包部署了
```bash
mvn package
```
target/hello-springboot-0.0.1-SNAPSHOT.jar
把生成这个打包好的jar文件拷贝到要部署的地方，运行下面命令就完成部署了
```bash
java -jar hello-springboot-0.0.1-SNAPSHOT.jar
```
