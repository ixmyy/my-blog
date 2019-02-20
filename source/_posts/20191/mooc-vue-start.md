---
title: 慕课网vue2.5入门 学习笔记
toc: true
date: 2019-02-19 10:07:00
tags: vue
categories: 笔记本
---

慕课网学习笔记；
[视频地址](https://www.imooc.com/learn/980)
[demo地址](https://www.imooc.com/learn/980)

<!-- more -->

# 一、Vue 基础语法

## 模板、实例、挂载点

## 数据、事件、方法

- 指令 :

```html
{{}}

v-text

v-html

content

```

- `v-on:click` 简写：`@click=handleClick`

`method:{ handleClick:function(){...}}`

## 属性绑定、双向数据绑定

- 属性绑定：`v-bind` 缩写：`:title="title"`

e.g. `v-bind:title="'llchen'+title"` 双引号里面是表达式

- 双向绑定： `v-model="content"`

## 计算属性、侦听器

- `computed:{ function(){ } }` 依赖属性不变，会使用缓存

- `watch:{ fucntion(){ } }`  数据变化时

## `v-if`、`v-show` 、`v-for`

- `v-if="show"`  标签从dom移除

- `v-show="show"`  隐藏：display:none

- `<li v-for="(item，index) of list" :key="index">{{item}}</li>`

-  > key可以提高渲染效率，key的值不能相同

# 二、Vue中的组件

## TdoList功能开发

```html
        <div id="root">
		<div>
		 	<input v-model="inputValue"/>
		 	<button @click="handleClick">提交</button>
		</div>
		<div>
		 	<ul>
		 		<li v-for="(item,index) of list" :key="index">{{item}}</li>
		 	</ul>
		</div>
		</div>
		<script>
	     new Vue({
	     	el:"#root",
	     	data:{
	     		inputValue:'',
	     		list:[]
	     	},
	     	methods:{
	     		handleClick:function(){
	     			this.list.push(this.inputValue);
	     			this.inputValue='';
	     		}
	     	}
	     })
		</script>
```

## todolist组件拆分

组件：页面上的某个部分

`:content` 通过`属性`的形式给子组件传参

`props` 字组件接收内容，再模板上显示

```html

        <div id="root">
    		<div>
    		 	<input v-model="inputValue"/>
    		 	<button @click="handleClick">提交</button>
    		</div>
    		<div>
    		 	<ul>
    		 		<todo-item v-for="(item,index) of list"
    		 		:key="index"
    		 		:content="item"
    		 		>
    		 		 </todo-item>
    		 	</ul>
    		</div>
		</div>
		<script>
		<!--定义全局组件：-->
        Vue.component('todo-item',{
            props:['content'],
            template:'<li>{{content}}</li>',
            methods:{
                handleClick:function(){
                    
                }
            }
        })
        <!--定义局部组件：-->
        var TodoItem={
            template:'<li>item</li>'
        }
	     new Vue({
	     	el:"#root",
	     	data:{
	     		inputValue:'',
	     		list:[]
	     	},
	     	<!--注册局部组件:-->
            components:{
                'todo-item':TodoItem
            }

	     	methods:{
	     		handleClick:function(){
	     			this.list.push(this.inputValue);
	     			this.inputValue='';
	     		}
	     	}
	     })
		</script>
```

## 组件与实例的关系

每一个组件都是一个实例，vue项目是由一个个实例构建而成的

每个组件可以写methods、data等

根实例没有template，会去寻找挂载点，把挂载点下面所有的dom标签作为实例模板。

```js
Vue.component('todo-item',{
    props:['content'],
    template:'<li>{{content}}</li>',
    methos：{}
    ...
})
```

## 实现todolist的删除功能  (子组件和父组件通信)

通过属性的形式给子组件传参

父组件通过发布订阅

```html

        <div id="root">
    		<div>
    		 	<input v-model="inputValue"/>
    		 	<button @click="handleClick">提交</button>
    		</div>
    		<div>
    		 	<ul>
    		 		<todo-item v-for="(item,index) of list"
    		 		:key="index"
    		 		:content="item"
    		 		:index="index"
    		 		@delete="handleDelete"
    		 		>
    		 		 </todo-item>
    		 	</ul>
    		</div>
		</div>
		<script>
		<!--定义全局组件：-->
        Vue.component('todo-item',{
            props:['content','index'],
            template:'<li @click="handleClick">{{content}}</li>',
            methods:{
                handleClick:function(){
                    this.$emit('delete',this.index)
                }
            }
        })
	    new Vue({
	     	el:"#root",
	     	data:{
	     		inputValue:'',
	     		list:[]
	     	},
	     	methods:{
	     		handleClick:function(){
	     			this.list.push(this.inputValue);
	     			this.inputValue='';
	     		},
	     		handleDelete:function(index){
	     		    this.list.splice(index,1)
	     		}
	     	}
	     })
		</script>
```

# Vue-cli的使用

## Vue-cli的简介与使用

- 安装

[Vue CLI 3 安装](https://cli.vuejs.org/zh/guide/installation.html)

- 使用

- 优势：

1 、使用ES6，可以打包成ES5

2 、单文件组件编码模式：模板、逻辑、样式

## 使用vue-cli开发TodoList

## 全局样式与局部样式

scoped修饰符：作用域为当前组件
