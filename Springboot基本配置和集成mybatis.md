---
title: Spring boot 基本配置和集成mybatis
date: 2019-01-27 17:07:00
tags:
---

#Spring boot 基本配置和集成mybatis
##迟来的Spring Boot学习序列（二）

* 真实的项目里我们需要考虑打包部署项目到不同的运行环境时配置信息不同的问题，比如开发环境，测试环境和生成环境，它们连接的数据库等中间件资源的配置信息是不同的，我们一般这么怎么处理，spring boot有很方便的支持
* 之前的配置文件是空的，我们来加点东西，
* *application.properties*
```xml 
info.app_name=My First Spring Boot Application
info.app_version=1.0.0

server.port=8080
server.servlet.context-path=/helle-springboot/
spring.profiles.active=dev

#mybatis
mybatis.mapperLocations=classpath:mapper/*Mapper.xml
mybatis.typeAliasesPackage=io.github.leenkhan.hellospringboot.domain
```
下面解释一下

* 新建三个空白配置文件application-dev.properties，application-test.properties，application-product.properties，加到application.properties同级目录下，，有关mybatis的mapper xml文件映射关系是各个环境里是不变的，应该配置在application.properties里，内容如下

*  *application-dev.properties*
```xml
#DataSource
spring.datasource.url=jdbc:mysql://localhost:3306/hellespring
spring.datasource.username=test
spring.datasource.password=123456
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
# use druid datasource
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource

#log
logging.level.root=INFO
logging.level.io.github.leenkhan.hellospringboot.mapper=TRACE
```

* jar包依赖 pom.xml
```xml
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<version>5.1.30</version>
</dependency>

<dependency>
	<groupId>com.alibaba</groupId>
	<artifactId>druid</artifactId>
	<version>1.1.0</version>
</dependency>

<dependency>
	<groupId>org.mybatis.spring.boot</groupId>
	<artifactId>mybatis-spring-boot-starter</artifactId>
	<version>1.3.2</version>
</dependency>
```
分别是mysql驱动、阿里的开源数据库连接池、springboot整合mybatis的依赖


* *City.java*
```java
/**
 *    Copyright 2015-2016 the original author or authors.
 *
 *    Licensed under the Apache License, Version 2.0 (the "License");
 *    you may not use this file except in compliance with the License.
 *    You may obtain a copy of the License at
 *
 *       http://www.apache.org/licenses/LICENSE-2.0
 *
 *    Unless required by applicable law or agreed to in writing, software
 *    distributed under the License is distributed on an "AS IS" BASIS,
 *    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 *    See the License for the specific language governing permissions and
 *    limitations under the License.
 */
package io.github.leenkhan.hellospringboot.domain;

import java.io.Serializable;

/**
 * @author Eddú Meléndez
 */
public class City implements Serializable {

	private static final long serialVersionUID = 1L;

	private Integer cityid;

	private String cityname;

	private Integer prvid;

	private String code;

	private Integer delflag;

	private String prvname;

	public static long getSerialVersionUID() {
		return serialVersionUID;
	}

	public Integer getCityid() {
		return cityid;
	}

	public void setCityid(Integer cityid) {
		this.cityid = cityid;
	}

	public String getCityname() {
		return cityname;
	}

	public void setCityname(String cityname) {
		this.cityname = cityname;
	}

	public Integer getPrvid() {
		return prvid;
	}

	public void setPrvid(Integer prvid) {
		this.prvid = prvid;
	}

	public String getCode() {
		return code;
	}

	public void setCode(String code) {
		this.code = code;
	}

	public Integer getDelflag() {
		return delflag;
	}

	public void setDelflag(Integer delflag) {
		this.delflag = delflag;
	}

	public String getPrvname() {
		return prvname;
	}

	public void setPrvname(String prvname) {
		this.prvname = prvname;
	}
}

```
* *CityMapper.java*
```java
package io.github.leenkhan.hellospringboot.mapper;

import io.github.leenkhan.hellospringboot.domain.City;
import org.apache.ibatis.annotations.Mapper;


public interface CityMapper {
    City getCity(Integer id);
}

```
* Mapper xml文件放置在src/main/resources/mapper/目录下
* *CityMapper.xml*
```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="io.github.leenkhan.hellospringboot.mapper.CityMapper">
    <select id="getCity" resultType="City">
        select * from city where CITYID = #{id}
    </select>
</mapper>

```

* *IndexController.java*
```java
package io.github.leenkhan.hellospringboot.controller;

import io.github.leenkhan.hellospringboot.domain.City;
import io.github.leenkhan.hellospringboot.mapper.CityMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;


import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@RestController
public class IndexController {
    @Autowired
    private CityMapper cityMapper;

    @RequestMapping("/index")
    @ResponseBody
    public String index(){
        return "hello spring boot";
    }

    @GetMapping("/json")
    @ResponseBody
    public Map<String,Object> jsonApi(){
        Map<String,Object> map  = new HashMap<>();
        map.put("code",0);
        map.put("message","success");

        List<String> list = new ArrayList<>();
        for(int i=0;i<100;i++){
            list.add(i+"");
        }

        City city = cityMapper.getCity(10);
        map.put("data",city);
        return map;
    }
}

```

sql
```sql
CREATE TABLE `city` (
  `CITYID` int(11) NOT NULL AUTO_INCREMENT,
  `CITYNAME` varchar(64) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `PRVID` int(11) DEFAULT NULL,
  `CODE` varchar(64) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `DELFLAG` int(1) DEFAULT NULL COMMENT '删除标志：\r\n            0---不删除\r\n            1---删除',
  `PRVNAME` varchar(64) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '行政省名称',
  PRIMARY KEY (`CITYID`),
  KEY `IDX_CITY_PRVID` (`PRVID`)
) ENGINE=InnoDB AUTO_INCREMENT=345 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

* 查看运行结果
http://localhost:8080/helle-springboot/json

