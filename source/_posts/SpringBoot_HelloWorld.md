---
title: SpringBoot_HelloWorld
date: 2018-03-07 16:46:38
categories: Java
tags:
    - SpringBoot
---

搭建一个HelloWorld工程学会使用SpringBoot实现简单的请求响应操作，可以自己写接口调试

<!-- more -->

### 添加依赖

 	<groupId>spring-boot-sample</groupId>
    <artifactId>springboot</artifactId>
    <version>1.0-SNAPSHOT</version>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.0.BUILD-SNAPSHOT</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <!-- Package as an executable jar -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <!-- Add Spring repositories -->
    <!-- (you don't need this if you are using a .RELEASE version) -->
    <repositories>
        <repository>
            <id>spring-snapshots</id>
            <url>https://repo.spring.io/snapshot</url>
            <snapshots><enabled>true</enabled></snapshots>
        </repository>
        <repository>
            <id>spring-milestones</id>
            <url>https://repo.spring.io/milestone</url>
        </repository>
    </repositories>
    <pluginRepositories>
        <pluginRepository>
            <id>spring-snapshots</id>
            <url>https://repo.spring.io/snapshot</url>
        </pluginRepository>
        <pluginRepository>
            <id>spring-milestones</id>
            <url>https://repo.spring.io/milestone</url>
        </pluginRepository>
    </pluginRepositories>

### 配置
使用注解**@EnableAutoConfiguration** 实现自动配置
使用注解**@RestController** 配置控制器
使用注解**@RequestMapping("/")**配置映射

### 运行

    public static void main(String[] args) {
        SpringApplication.run(HellApplication.class, args);
    }

### 打包

使用命令 **mvn package**, 会在target目录下生成jar文件
执行使用命令 **java -jar xx.jar**

# 
