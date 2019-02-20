---
title: 慕课网 Vue2.5开发去哪儿网App 从零基础入门到实战项目
toc: true
date: 2019-02-19 16:07:00
tags: vue
categories: 笔记本
---

慕课网 Vue2.5开发去哪儿网App 学习笔记
[视频地址](https://coding.imooc.com/class/203.html)

<!-- more -->

# 一、课程介绍

vue.js优势：

上手简单，容易使用

NUXT框架：vue服务器端渲染

WEEX：使用vue语法编写原生APP

vue的社区活跃，代码问题基本上都可以在社区中查到

学习前提：

js ES6 webpack npm

# 二、Vue初探

## 2.4 MVVM模式

只需要关注V层和M层 ，操作数据（M层)，页面改变（V层),vue起VM层的作用，

jquery（MVP设计模式）面向DOM开发

MVVM面向数据开发，优点：代码显著减少（30%~70%）

## 2.5 前端组件化

把页面分隔为一个一个部分(组件)，使其逻辑简单，维护方便

## 2.6 组件化思想修改todolist

封装TodoList、TodoItem。

## 2.7 简单的组件间传值

父组件>子组件：

父：`v-bind:content="item" v-bind:index="index"`

子：`props:['content','index'],`

子组件>父组件：

子：`this.$emit('delete',this.index)`

父：`@delete="handleDelete"`

# 三、Vue基础知识

## 3.1 Vue实例

创建Vue实例：

```html
<!--挂载点-->
<div id="#root">
     <div @click="handleClick"> 
        <!--  vue会到数据里面寻找`message`，用数据替换掉`插值表达式`-->
        {{message}}
     </div>
     <item></item>
</div>

<script>

vue.component('item',{
    template:'<div>hello world</div>'
})

var vm =new Vue({  //定义vue实例。此实例为根实例。组件=实例
    el:'#root'    //让实例接管#root标签里dom显示
    data:{
        message:'hello world'
    },
    methods:{
        handleClick:function(){
            alert("hello")
        }
    }
})
</script>
```

`vm.$destroy()`:销毁实例，实例绑定的事件不会销毁

## 3.2 Vue实例的生命周期钩子

每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会。

生命周期函数不放在method里

```html
<!--挂载点-->
<div id="#app">

</div>

<script>
//声明周期函数：vue实例在某一个时间点会自动执行的函数
var vm =new Vue({
    el:'#app',
    template:"<div>  hello world</div>",

    beforeCreate:function(){
        console.log("before Create");
    },
    created:function(){
        console.log("created");
    },
    //即将挂载之前
    befoerMount:function(){
        console.log("beforeMount");
    },
    //模板结合实例生成的，会显示到页面之上；
    //页面挂载之后：
    mounted:function(){
        console.log("mounted")
    },
    //实例还没有被销毁
    beforeDestroy:function(){
        console.log("beforeDestroy")
    },
    //实例销毁之后
    destroy:function(){
        console.log("destroy")
    },
    //数据发生改变，还没有重新渲染之前
    beforeUpated:function(){
        console.log("beforeUpated")
    },
    //重新渲染之后
    update:function(){
        console.log("update")
    },
})
</script>
```

[官方文档](https://cn.vuejs.org/v2/guide/instance.html)

## 3.3 vue模板语法

插值表达式：把数据变量显示在页面上，语法内容中可以写js表达式,不仅仅可以写变量

|  插值表达式    |    用法 | -  |
| :--: | :--:| :--: |
|{{name}}   | 直接输出 |  -   |
| v-text     |   直接输出 |  -  |
| v-html       |    解析之后输出 | -  |

## 3.4 计算属性、方法、侦听器

（1）**计算属性**
内置缓存：fullName依赖的firstName和lastName不改变时，fullName使用缓存，提高效率

```html
<div id="#app">
    {{fullName}}
    {{age}}
</div>

<script>
var vm =new Vue({
    el:'#app'
    data:{
        firstName:"Lilin",
        lastName:"Chen"，
        age:"22"
    },
    //计算属性
    computed:{
        fullName:function(){
          console.log("计算了一次")
            retuen this.firstName+" "+this.lastName;
        }
    }
})
</script>
```




















# 四、基础知识精讲

# 五 、基础知识精讲

# 六、Vue项目实战
# 七、Vue项目实战
# 八、Vue项目实战
# 九、Vue项目实战
#十、项目上线流程及后续学习指南
