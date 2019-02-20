---
title: 搭建Vue脚手架
toc: true
date: 2018-12-25 09:15:15
tags: vue
categories: javascript
---

# 一、项目的搭建

## npm来构建vue项目

<!-- more -->

- 安装node.js
    <https://nodejs.org/en/download/>
- (安装淘宝镜像)
    由于 npm 安装速度慢，所以本文使用淘宝镜像及其命令 cnpm进行安装，输入`$ npm install -g cnpm --registry=https://registry.npm.taobao.org` 安装淘宝镜像。
- 安装Vue
    输入`$ cnpm install vue` ，回车等待终端给出响应，并输入`$ vue -V`检查是否安装成功。
- 安装全局vue-cli脚手架
    输入`$ cnpm install --global vue-cli`,安装全局==vue-cli==脚手架，用于快速搭建大型单页应用。
- 安装 vue 路由模块 vue-router 和网络请求模块 vue-resource
    输入：`cnpm install vue-router vue-resource --save`。
- 构建vue项目
    使用`vue init webpack Vue-youzan` 直接创建一个基于 webpack 模板的新项目，注意不建议使用带-名称的项目名！
    根据提示完成操作,进入项目，安装并运行：
    ![“vue”](/images/20184/vuedev1.png)  
    `Install vue-router? (Y/n) -------------- 是否安装Vue路由，也就是以后是spa（但页面应用需要的模块）
    Use ESLint to lint your code? (Y/n) n ----------是否启用eslint检测规则，这里建议选no`

    也可在vscode终端中运行
    ![“vscode中”](/images/20184/vuedev2.png)  
    成功执行以上命令后访问 <http://localhost:8080/>，输出结果如下所示：
    ![“vue欢迎页”](/images/20184/vuedev3.png)  

- 生成目录的作用
    ![“目录的作用”](/images/20184/vuedev5.png)  


## 使用yarn

- 安装yarn,安装之后输入`yarn --version`查看是否成功
    <https://yarn.bootcss.com/docs/install/#windows-stable>
- yarn 切换、设置镜像源
    查看一下当前源
    `yarn config get registry`
    切换为淘宝源
    `yarn config set registry https://registry.npm.taobao.org`
    切换为自带的
    `yarn config set registry https://registry.yarnpkg.com`
- 进入项目，运行
    ![“yarn运行”](/images/20184/vuedev4.png)  
- 常用命令
    1、安装 package.json 中的所有文件
    `yarn` 或者 `yarn install` /  `npm install`
    yarn install 安装时，如果 node_modules 中有相应的包则不会重新下载 --force 可以强制重新下载安装
     `yarn install --force`/`npm install --force`
    2、`yarn run`

## 运行报错

- ![“报错信息1”](/images/20184/vuedev6.png)  
    原因：镜像源切换错误
    解决方法：重新设置镜像源
- ![“报错信息2”](/images/20184/vuedev7.png)  
    原因：报错信息很清楚：ChromeDriver出问题
    解决方法：找一个国内的镜像源安装
    `npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver`
    完成
    ![“报错信息2解决”](/images/20184/vuedev8.png)  

- >This is probably not a problem with npm,there is likely additional logging output

    解决方法：
    输入npm install 或 cnpm install 后，再次启动
- >This dependency was not found:*@/assets

    解决方法：
    在build文件夹中找到webpack.base.conf.js，找到文件中的resolve中的alias，在里边加上要配置的路径

    ```js
    const pathMap = {
    vue$: "vue/dist/vue.esm.js",
    "@": resolve("src"),
    static: resolve("static")
    };
    ```

# 二、项目主要组成

## index.html
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>test</title>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```

## main.js

主页面配置的主入口

```js
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'  /* 这里是引入vue文件 */
import App from './App'  /* 这里是引入同目录下的App.vue模块 */
import router from './router'  /* 这里是引入vue的路由 */

Vue.config.productionTip = false
/* eslint-disable no-new */
new Vue({
  el: '#app',  /* 定义作用范围就是index.html里的id为app的范围内 */
  router,  /* 引入路由 */
  components: { App },   /* 给Vue实例初始一个App组件作为template 相当于默认组件 */
  template: '<App/>'   /* 注册引入的组件App.vue */
})

```

## index.js

```js
 /* 引用vue文件 */
import Vue from 'vue'
 /* 引用vue路由模块，并赋值给变量Router */
import Router from 'vue-router' 
 /* HelloWorld.HelloWorld,这里是“@”相当于“../” */
import HelloWorld from '@/components/HelloWorld' 

Vue.use(Router)   /* 使用路由 */

export default new Router({
  routes: [  /* 进行路由配置，规定“/”引入到HelloWorld组件 */
    {
      path: '/', 
      name: 'HelloWorld',
      component: HelloWorld /* 注册HelloWorld组件 */
    }
  ]
})
```

## APP.vue

```html
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <!--  展示路由页面内容，如果想用跳转就用<router-link to='xxx'></router-link> -->
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

## HelloWorld.vue

```html
...
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      /* 这里是数据，一定记住数据一定要放data中然后用return返回 */
      msg: 'Welcome to Your Vue.js App' 
    } 
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
 <!--scoped的意思是这里的样式只对当前页面有效不会影响其他页面，
 还有可以设置lang="scss"就是支持css预编译，也就是支持sass或者less  -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
...

```

