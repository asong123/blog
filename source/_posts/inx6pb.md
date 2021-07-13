---
title: Debug Linux上跑的程序
urlname: inx6pb
date: '2021-07-12 14:15:43 +0800'
author: 阿松
top: true
hide: false
cover: false
toc: false
mathjax: false
summary: Debug Linux上跑的程序
categories: linux
tags:
  - Debug
  - 后端
  - linux
abbrlink: 48541
---

# DEBUG linux 上跑的程序

## 1.1 步骤

> 参考 15

​

第一步，idea 上新建 Remote
![image.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1591411315153-e9b351b7-d546-4725-8d01-9c55e2a622a0.png#height=690&id=vP4oT&margin=%5Bobject%20Object%5D&name=image.png&originHeight=690&originWidth=1174&originalType=binary∶=1&size=86617&status=done&style=none&width=1174)
第二步，修改配置增加启动参数

> FROM java:8
> MAINTAINER liulijun
> ADD toolkit-docker-0.0.1-SNAPSHOT.jar toolkit-docker.jar
> EXPOSE 8080
> ENTRYPOINT ["java","-Xdebug","-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=8999","-jar","toolkit-docker.jar","> /log/app.log"]
