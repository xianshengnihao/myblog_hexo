---
title: 二、xml方式初始话Spring容器详解
tags:
  - Spring容器初始化
  - xml方式
categories:
  - Spring
  - 源码
top: true
cover: true
mathjax: false
abbrlink: 68eba84b
date: 2021-10-10 19:22:53
summary: 类路径获取配置文件的方式初始Spring容器
---

### [1] 加载Spring容器的核心方法

`AbstractApplicationContext.refresh()`方法是Spring容器启动过程中的核心方法，不论以何种方式加载Spring容器都必须执行该方法。

![](https://p.pstatp.com/origin/pgc-image/c26d3f9a2c33496db6fb29649c0422ab)

1、首先看下refresh() =>obtainFreshBeanFactory()方法。该方法大致流程为

1. 调用createBeanFactory（）完成BeanFactory的创建
2. XML解析：首先创建`XmlBeanDefinitionReader`解析器，解析器调用`loadBeanDefinitions`方法完成Xml的解析然后封装成为`BeanDefinition`对象。（`BeanDefinition`对象即为Bean的定义信息，里面包含了Bean的name，class，以及属性的初始值等等Bean的信息。）`loadBeanDefinitions`本质上就是以流的方式将文件加载xml文件，并封装成为InputResource对象，然后通过JDK的dom4j将InputResource对象转换封装成Document对象，拿到Document对象以后即可对xml的每个节点进行操作。
3. 默认标签的解析：(import、Bean等)

------
没组织好语言。。想想怎么表达一下，源码确实不好描述，重要的是理解
