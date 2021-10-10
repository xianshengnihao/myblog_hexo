---
title: 一、Spring入口和xml解析
tags:
  - Spring源码
  - Spring简介
  - Spring源码下载
categories:
  - Spring
  - 源码
top: true
cover: true
summary: Spring源码下载和入口解析
abbrlink: 374bf9da
date: 2021-10-08 17:28:05
---

### [1]Spring源码下载

1. `git clone -b v5.2.8.RELEASE https://github.com/spring-projects/spring-framework.git`

2. 安装配置Gradle，因为spring已经抛弃了maven全面使用Gradle来进行项目构建，安装配置自行百度

3. 进入到下载好的源码路径下执行gradle命令

```shell
gradlew:spring-oxm:compileTestJava
```

4. 用idea打开spring源码工程，在idea中安装插件kotlin，重启idea
5. 把编译好的源码导入到工程中。

### [2]将源码导入工程

1. ctrl+alt+shift+s 打开Project Structure 如图下图所示
   ![](https://p.pstatp.com/origin/pgc-image/ef1edbc6f6f54e439af989f724388a4b)

2. 选择 `org.springframework:spring-context:xxx.RELEASE` 然后点击右侧Source然后点击+号

   ![](https://p.pstatp.com/origin/pgc-image/5444c90f2b454ee5b8d820c4ed2616cc)

3. 然后去源码目录下找到对应的spring-context 点击OK

   ![](https://p.pstatp.com/origin/pgc-image/2ff237e9c57646fa86e1d61e3f788d18)

4. 指定classes文件路径，同样点击右侧Classes，点击+号然后选择源码目录下spring-context文件，点击选择文件下的build目录，然后选择libs下的jar包

   ![](https://p.pstatp.com/origin/pgc-image/c9e369cb0be141b08b257e002d1b681a)

> 注意：后续如果需要调试beanbao、Aop包等用相同的方法引入到自己的工程即可

### [3]导入jar依赖

> Spring中4个核心的jar

- Spring-context
- Srping-core
- Spring-beans
- Spring-expression

> 一个简单的spring工程，理论上只需要Spring-context包就够了，因为其本身就依赖了Spring-aop,Spring-beans,Spring-core 包

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>${spring.version}</version>
</dependency>
```

> 一个空的 spring 工程是不能打印日志的，要导入 spring 依赖的日志 jar

```xml
<dependency>
	<groupId>ch.qos.logback</groupId>
	<artifactId>logback-classic</artifactId>
	<version>LATEST</version>
</dependency>
```

### [4]Spring容器的加载方式

1.类路径获取配置文件加载Spring容器

```java
//xml方式启动spring容器
ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
```

2.通过文件系统绝对路径获取配置文件的方式加载Spring容器

```java
ApplicationContext applicationContext = new FileSystemXmlApplicationContext("E:\\IDEA project\\spring.xml");
```

3.无配置文件加载Spring容器，即纯注解的方式

```java
ApplicationContext applicationContext = new AnnotationConfigApplicationContext("com.xx.jack");
```

4.SpringBoot加载容器

```java
ApplicationContext applicationContext = new EmbeddedWebApplicationContext();
```

