---
layout: w
title: linux网络编程
date: 2018-05-23 19:54:56
tags:
---
```
print inet_ntoa(ntohl(htonl(inet_addr("127.0.0.1"))))
//将127.0.0.1转化为int，再转为网络字节序，再转回来主机字节序，再转化为ip地址
inet_addr()函数对应的头文件：
Winsock2.h (windows)
arpa/inet.h (Linux)
```