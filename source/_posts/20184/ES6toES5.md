---
title: ES6toES5
date: 2018-12-10 10:13:30
tags: 
   - ES6
categories: javascript
toc: true
---

将es6语法转换成es5语法
ECMAScript 6(ES6)的发展速度非常之快，但现代浏览器对ES6新特性支持度不高，所以要想在浏览器中直接使用ES6的新特性就得借助别的工具来实现。
<!-- more -->


##1、利用babel工具
###  1、新建工程初始化项目
新建工程文件夹这里起名叫做es6,然后在里面创建两个文件夹分别为src 、dist如下图：(src为待转换es6 js存放目录，dist为编译完成后的es5 js存放目录)

在src目录下新建一个js文件（这里起名叫做index.js），里面输入es6的代码：

 > npm  init   

![新建的测试项目](/images/20184/ec6toes52.png)

### 2、全局安装babel工具

> npm install -g babel-cli

安装转换包

> npm install --save-dev babel-preset-es2015 babel-cli

安装完成后，在package.json文件添加devDependencies

![package.json新增内容](/images/20184/ec6toes51.png) 


### 3、新建.babelrc
在项目根目录新建(.babelrc)文件输入如图所示代码：
![.babelrc新增内容](/images/20184/ec6toes53.png) 

### 4、转换
   终端输入如下命令：（babel  待转换路径/ --out-dir 转换后路径/）

我们这里是从src转换到dist目录下

> babel src --out-dir dist

现在我们dist目录下面就生成了编译后的js



<!-- ![测试](https://1024-1253939655.cos.ap-beijing.myqcloud.com/hexo_poem/poem.jpg)  -->
![.babelrc新增内容](/images/20184/ec6toes54.png) 

### 5、用命令操作

通过修改package.json里面的别名来实现编译  

**修改（"es6build":"babel src --out-dir dist"）**


![.babelrc新增内容](/images/20184/ec6toes55.png) 

以后你只需要如下命令就可以编译了

> npm run es6build




