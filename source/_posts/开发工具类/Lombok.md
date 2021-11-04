---
title: Lombok
date: 2021-11-04 01:14:36
---

## 什么是lombok

> Lombok项目是一个Java库，它会自动插入编辑器和构建工具中，Lombok提供了一组有用的注释，用来消除Java类中的大量样板代码。仅五个字符(@Data)就可以替换数百行代码从而产生干净，简洁且易于维护的Java类。

## 使用lombok

### 1.引入依赖

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
    <scope>provided</scope>
</dependency>
```

### 2.使用提供注解

#### @Data注解：

用在类上，自动给类提供 SET、 GET、 toString、hashCode、equals等方法

#### @Getter/@Setter: 

用在类上，自动给类提供 SET 或者 GET

#### @ToString:

 用在类上，自动给类提供 toString()方法

#### @AllArgsConstructor:

用在类上，自动给类提供 全参构造方法,如提供全参，则记得必须加上无参@NoArgsConstructor

#### @NoArgsConstructor:

用在类上，自动给类提供 无参构造方法

#### @Accessors(chain = true)：

用在类上，用来给类的setter方法开启链式调用,其中chain属性决定是否开启链式
    user.set(..).set(..).set()......

#### @slf4j4:

用在类上,用来快速给类定义一个日志变量

相当于添加了变量private Logger log = LoggerFactory.getLogger(this.getClass());

## lombok原理

@Retention(RetentionPolicy.SOURCE),表示在编译成字节码的时候植入代码

## lombok安装

默认IDEA没有安装lombok插件
