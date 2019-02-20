---
title: 微信小程序入门
toc: true
date: 2019-01-16 15:15:06
tags:
categories: 开发者手册
---

总结一下小程序开发文档，

<!-- more  -->

[小程序介绍](https://developers.weixin.qq.com/miniprogram/introduction/#%E4%BA%A7%E5%93%81%E5%AE%9A%E4%BD%8D%E5%8F%8A%E5%8A%9F%E8%83%BD%E4%BB%8B%E7%BB%8D)

[小程序开发](https://developers.weixin.qq.com/miniprogram/dev/)

# 一、基础

## 1、代码构成

`.json` 后缀的 `JSON` 配置文件
`.wxml` 后缀的 `WXML` 模板文件
`.wxss` 后缀的 `WXSS` 样式文件
`.js` 后缀的 `JS` 脚本逻辑文件

![“代码目录”](/images/20191/wechat1.png)  

### JSON 配置

- `app.json`:小程序的全局配置，包括了小程序的所有页面路径、界面表现、网络超时时间、底部 tab 等。
    [小程序配置](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html)
- `project.config.json`:重新安装工具或者换电脑工作时，只要载入同一个项目的代码包,开发者工具就自动会帮你恢复到当时你开发项目时的个性化配置
    [开发者工具配置](https://developers.weixin.qq.com/miniprogram/dev/devtools/projectconfig.html)
- `page.json`:独立定义每个页面的一些属性
    [页面配置](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#%E9%A1%B5%E9%9D%A2%E9%85%8D%E7%BD%AE)

### WXML 模板

充当的就是类似 HTML 的角色
不同点：

- 1、标签名字略不同，根据基础的标签组合出不一样的组件
- 2、MVVM 的开发模式

参考：[WXML](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/index.html)

### WXSS 样式

具有 CSS 大部分的特性

不同点：

- 1、新增了尺寸单位rpx
- 2、提供了全局的样式和局部样式，app.wxss会作用于所有页面，page.wxss仅对当前页面生效。

参考：[WXSS](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxss.html)

### JS 交互逻辑

和一般的js相同

参考：[WXML - 事件](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html)

## 2、小程序能力

### 小程序的启动

- 1、把整个小程序的代码包下载到本地
- 2、通过 app.json 的 pages 字段知道你当前小程序的所有页面路径:
- 3、写在 pages 字段的第一个页面就是首页，首页的代码装载进来
- 4、小程序启动之后，在 app.js 定义的 App 实例的 onLaunch 回调会被执行

### 组件

小程序提供了丰富的基础组件给开发者，开发者可以像搭积木一样，组合各种组件拼合成自己的小程序。

参考：[小程序的组件](https://developers.weixin.qq.com/miniprogram/dev/component/)

### API

参考：[小程序的API](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/api.html)

## 3、发布上线

































# 廿、遇到的问题


## 1、引入Wafer2 快速开发 Demo 登录错误
[Wafer2 git地址](https://github.com/tencentyun/wafer2-startup/blob/master/README.md)

[解决方法：](https://github.com/tencentyun/wafer2-quickstart/issues/13)

## 2、app.json Expecting 'STRING','NUMBER','NULL','TRUE','FALSE','{','[',']', got INVALID

原因是在app.json中不能有注释，将上面注释部分去掉，就可以了



