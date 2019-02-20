---
title: vue入门踩坑
toc: true
date: 2019-01-29 14:45:32
tags: vue
categories: javascript
---

vue最入门

<!-- more  -->

# [Vue warn]: Duplicate keys detected: '1'. This may cause an update error.

```html
<blog-post v-for="post in posts" v-bind:key="post.id" v-bind:title="post.title"></blog-post>
<blockquote>单个根元素</blockquote>
<blog-post-1 v-for="post in posts" v-bind:key="post.id" v-bind:post="post"></blog-post-1>
```

上面代码会出现 [Vue warn]: Duplicate keys detected: '1'. This may cause an update error. 错误，key 属性冲突导致

![“Duplicate keys detected: '1'. This may cause an update error”](/images/20191/vueerror1.png)  

修改为：

```html
<blog-post v-for="post in posts" v-bind:key="post.id" v-bind:title="post.title"></blog-post>
<blockquote>单个根元素</blockquote>
<blog-post-1 v-for="post in posts" v-bind:key="post.id+ '-label1'" v-bind:post="post"></blog-post-1>
```

# Unknown custom element: <component-a> - did you register the component correctly? For recursive components, make sure to provide the "name" option.

原因：组件没有注册；可能：

- 名字写错

- components位置写错

- 当使用 PascalCase (首字母大写命名) 定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说 `<my-component-name>` 和 `<MyComponentName>` 都是可接受的。注意，尽管如此，直接在 DOM (即非字符串的模板) 中使用时只有 kebab-case 是有效的。

