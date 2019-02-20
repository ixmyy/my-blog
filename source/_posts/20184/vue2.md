---
title: vue学习（二）  组件
date: 2018-12-21 14:33:13
tags: vue
categories:  javascript
cover: /images/cover/vue-router.png

---

# 深入了解组件

## 组件注册

```js
Vue.component('my-component-name', { /* ... */ })

//字母全小写且必须包含一个连字符( W3C 规范中的自定义组件名 )
```

- **组件名**

组件名是 Vue.component 的第一个参数。

可以使用 **kebab-case**或 **PascalCase**方式

> 使用 **PascalCase** 定义时，引用这个自定义元素时两种命名法都可以使用。

- **全局注册**

```js
Vue.component('my-component-name', {
  // ... 选项 ...
})
```

注册之后可以用在任何新创建的 Vue 根实例 (new Vue) 的模板中

- **局部注册**

全局注册会造成了用户下载的 JavaScript 的无谓的增加。比如即便你已经不再使用一个组件了，它仍然会被包含在你最终的构建结果中。

在这些情况下，你可以通过一个普通的 JavaScript 对象来定义组件：

```js
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
```

然后在 components 选项中定义你想要使用的组件：

```js
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

## Prop

Prop 是你可以在组件上注册的一些自定义特性。当一个值传递给一个 prop 特性的时候，它就变成了那个组件实例的一个属性。

- **Prop 的大小写 (camelCase vs kebab-case)**

HTML 中的特性名大小写不敏感，浏览器会把所有大写字符解释为小写字符

- **Prop 类型**

可以以对象形式列出 prop，这些属性的名称和值分别是 prop 各自的名称和类型：

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object
}
```

- **传递静态或动态 Prop**

任何类型的值都可以传给一个 prop。

```html
数字：
<!-- 即便 `42` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:likes="42"></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:likes="post.likes"></blog-post>

布尔值：
<blog-post v-bind:is-published="false"></blog-post>

数组：
<blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>

对象：
<blog-post
  v-bind:author="{
    name: 'Veronica',
    company: 'Veridian Dynamics'
  }"
></blog-post>

一个对象的所有属性：
<blog-post v-bind="post"></blog-post>
等价于
<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>
```

- **单向数据流**

父级 prop 的更新会向下流动到子组件中，但是反过来则不行。

- **Prop 验证**

可以为 props 中的值提供一个带有验证需求的对象，而不是一个字符串数组。

``` js
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 匹配任何类型)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

## 自定义事件

