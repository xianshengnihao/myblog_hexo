---
title: TCP三次握手
tags: 面试题
categories: 计算机网络
abbrlink: 3d85440b
date: 2021-10-06 21:12:25
---
# TCP三次握手

## OSI七层模型和TCP/IP模型之间的对应关系：

![网络模型](https://p.pstatp.com/origin/pgc-image/514a95a89c404beebcfc20910fbad80b)
## TCP建立连接的三次握手（req为一个序列号）

- **一次握手：客户端向服务端发送建立连接的请求：SYN=1  req=j**
- **二次握手：服务端收到客户端请求并向客户端响应请求 SYN=1 ACK=1 ack=j+1 req=K**
- **三次握手：客户端收到服务端的响应并且向服务端发送确认消息： ACK=1 ack=K+1**

![三次握手  ](https://p.pstatp.com/origin/pgc-image/d482c366d7fc4b03b270130bf54ed44b)  