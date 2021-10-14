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



### [2]xml文件加载流程

`ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();`  该方法主要进行xml的解析工作，流程如下：

1. **创建`XMLBeanDefinitionReader`对象**

![](https://p.pstatp.com/origin/pgc-image/3c68af85b34f475fb1c54c302f5544be)

2. **通过 `XMLBeanDefinitionReader`对象加载配置文件**  

![](https://590233ee4fbb3.cdn.sohucs.com/auto/1-auto5fec94fc6861496aa2a7ee31757bbb85)

3.  **以流的方式将读取配置文件分装成Resource对象**

![](https://p.pstatp.com/origin/pgc-image/bf405b89f6da4bcb88f750075a3f3541)

4. **将Resource对象转换为带编码的`EncodedResource`对象**

![](https://590233ee4fbb3.cdn.sohucs.com/auto/1-auto21eab775a3484c5e854b2587d7ab0f54)

5. **将`EncodedResource`对象转换为`InputResource`对象**

![](https://590233ee4fbb3.cdn.sohucs.com/auto/1-autob60ff3d7cb444a6a98e55ed9636f8c09)

6. **根据加载的配置文件把配置文件封装成 document 对象**  

![](https://590233ee4fbb3.cdn.sohucs.com/auto/1-auto08bdc11dfe1946f787c353d1b062dd5d)

7. **标签解析 `parseDefaultElement(ele, delegate)`负责常规标签解析  `delegate.parseCustomElement(ele)`负责自定义标签解析**  

![](https://590233ee4fbb3.cdn.sohucs.com/auto/1-auto0857bf1ca710480fb406c562e0adf49e)

> 重点：解析xml标签的最终目的是将Bean的信息封装成为一个BeanDefinition对象并且将BeanDefinition 对象添加到缓存中（BeanDefinitionMap）BeanDefinition即为Bean的定义信息，用来描述Bean的各项信息，比如Name，Class，需要依赖注入的属性等等。关于BeanDefinition请看 {% post_link BeanDefinition详解 BeanDefinition详细描述%}

###  [3] 自定义标签解析

1. **获取自定义标签的namespace命名空间，例如：**

```java
//http://www.springframework.org/schema/context
String namespaceUri = getNamespaceURI(ele);
```

2. **根据命令空间获取 NamespaceHandler 对象**。

   ```java
   NamespaceHandler handler = this.readerContext.getNamespaceHandlerResolver().resolve(namespaceUri);
   ```

   > 解析过程：
   >
   > * 解析spring.handlers文件中的数据并且将将namespace和NamespaceHandler简历映射关系
   > * 根据当前namespaceUri获取到到NamespaceHandler的权限的名称
   > * 使用反射的方式创建NamespaceHandler对象
   > * 调用NamespaceHandler对象的init方法

   ```java
   //获取spring中所有jar包里面的 "META-INF/spring.handlers"文件，并且建立映射关系
   Map<String, Object> handlerMappings = getHandlerMappings();
   //根据namespaceUri：http://www.springframework.org/schema/context，获取到这个命名空间的处理类
   Object handlerOrClassName = handlerMappings.get(namespaceUri);
   if (handlerOrClassName == null) {
   	return null;
   }
   else if (handlerOrClassName instanceof NamespaceHandler) {
   	return (NamespaceHandler) handlerOrClassName;
   }
   else {
   	String className = (String) handlerOrClassName;
   	try {
   		Class<?> handlerClass = ClassUtils.forName(className, this.classLoader);
   		if (!NamespaceHandler.class.isAssignableFrom(handlerClass)) {
   			throw new FatalBeanException("Class [" + className + "] for namespace [" + namespaceUri +
   					"] does not implement the [" + NamespaceHandler.class.getName() + "] interface");
   		}
           //反射创建NamespaceHandler对象
   		NamespaceHandler namespaceHandler = (NamespaceHandler) BeanUtils.instantiateClass(handlerClass);
   
   		//调用处理类的init方法，在init方法中完成标签元素解析类的注册
   		namespaceHandler.init();
   		handlerMappings.put(namespaceUri, namespaceHandler);
   		return namespaceHandler;
   	}
   	catch (ClassNotFoundException ex) {
   		throw new FatalBeanException("Could not find NamespaceHandler class [" + className +
   				"] for namespace [" + namespaceUri + "]", ex);
   	}
   	catch (LinkageError err) {
   		throw new FatalBeanException("Unresolvable class definition for NamespaceHandler class [" +
   				className + "] for namespace [" + namespaceUri + "]", err);
   	}
   }
   
   ```

3. **调用 parse 方法**

   ```java
    handler.parse(ele, new ParserContext(this.readerContext, this, containingBd));
   ```

> 扩展：spring.handler文件其实就是 namespaceUri 和类的完整限定名的映射。下面为spring-context包下的spring.handlers

```xml
http\://www.springframework.org/schema/context=org.springframework.context.config.ContextNamespaceHandler
http\://www.springframework.org/schema/jee=org.springframework.ejb.config.JeeNamespaceHandler
http\://www.springframework.org/schema/lang=org.springframework.scripting.config.LangNamespaceHandler
http\://www.springframework.org/schema/task=org.springframework.scheduling.config.TaskNamespaceHandler
http\://www.springframework.org/schema/cache=org.springframework.cache.config.CacheNamespaceHandler
```



### [4] XML整体流程图

![](https://p.pstatp.com/origin/pgc-image/74f4964dd48a4303bd432a167770b99e)
