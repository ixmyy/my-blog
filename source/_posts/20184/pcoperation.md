---
title: 电脑常用操作
date: 2018-12-07 16:00:05
tags:
categories: 开发者手册
toc: true
---

一些常用操作，杀进程、快捷键、装软件...

<!-- more -->

## Windows中杀死占用某个端口的进程
```
netstat -ano | findstr 80 //列出进程极其占用的端口，且包含 80
tasklist | findstr 9268
taskkill -PID <进程号> -F //强制关闭某个进程
```
## postman Cookie请求
![postman](/images/20184/postman1.png)

## idea快捷键
多光标：Alt+Shift+左键 

修改主题：Alt_Ctrl_S    Appearance & Behavior > Appearanc  Editor > Colors & Fonts

修改字体：Files -> Settings    Appearance & Behavior > Appearance

生成getter：Alt+Ins

导入包：手动：Alt+Enter      自动：settings-->editor-->general-->auto Import-->勾选

格式化：Ctrl+Alt+L

## plsql使用

#### 1、instantclient_11_2安装包 解压到 PLSQL Developer 12里面
![postman](/images/20184/plsql1.png)
#### 2、配置环境变量；
#### 3、3、地址输入：
![postman](/images/20184/plsql2.png)

