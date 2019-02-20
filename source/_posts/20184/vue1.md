---
title: vue学习（一）  vue基本
date: 2018-12-20 14:33:13
tags: vue
categories:  javascript
cover: /images/cover/vue-router.png

---

Vue.js 使用了基于 HTML 的模版语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。

Vue.js 的核心是一个允许你采用简洁的模板语法来声明式的将数据渲染进 DOM 的系统。

结合响应系统，在应用状态改变时， Vue 能够智能地计算出重新渲染组件的最小代价并应用到 DOM 操作上。
<!--more-->

# 模板语法

## 插值

|插值 | e.g. | 说明|
| :--------: | :--------:| :--: |
|文本 | \{\{ &nbsp;&nbsp;\}\} | ---|
|Html | v-html |使用 v-html 指令用于输出 html 代码：|
|属性 | v-bind  | 属性中的值应使用 v-bind 指令。|
|表达式 | {{5+5}} | 完全的 JavaScript 表达式支持。|

## 指令

指令是带有 v- 前缀的特殊属性。

|指令 | e.g. | 说明| 缩写
| :-------- | --------:| :--: | :--: |
|指令 | v-if="seen"|--- |--- |
|参数 | v-bind:href="url"| 参数在指令后以冒号指明。 |:href="url"|
|--- | v-on:click="doSth"  | 用于监听 DOM 事件：| @click="doSth"|
|修饰符 | v-on:submit.prevent="onSubmit"|指出一个指令应该以特殊方式绑定| ---|

## 用户输入

在 input 输入框中我们可以使用 ==v-model== 指令来实现双向数据绑定：

|用户输入 | e.g. | 说明|
| :-------- | --------:| :--: |
|---|v-model="message"|  指令用来在 input、select、text、checkbox、radio 等表单控件||元素上创建双向数据绑定，根据表单上的值，自动更新绑定的元素的值。|

## 过滤器

Vue.js 允许你自定义过滤器，被用作一些常见的文本格式化。由"管道符"指示, 

可以串联 、可以接受参数

```none
<!-- 在两个大括号中 -->
{{ message | capitalize }}

<!-- 在 v-bind 指令中 -->
<div v-bind:id="rawId | formatId"></div>
```

# 条件语句

- v-if
- v-else
- v-else-if
- v-show

```html
<h1 v-show="ok">Hello!</h1>
<div id="app">
    <div v-if="type === 'A'">
      A
    </div>
    <div v-else-if="type === 'B'">
      B
    </div>
    <div v-else-if="type === 'C'">
      C
    </div>
    <div v-else>
      Not A/B/C
    </div>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    type: 'C'
  }
})
</script>
```

# 循环语句

循环使用 v-for 指令。

v-for 指令需要以 site in sites 形式的特殊语法， sites 是源数据数组并且 site 是数组元素迭代的别名。

渲染一个列表：

```html
<div id="app">
  <ol>
    <li v-for="site in sites">
      {{ site.name }}
    </li>
  </ol>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    sites: [
      { name: 'Runoob' },
      { name: 'Google' },
      { name: 'Taobao' }
    ]
  }
})
</script>
```

v-for 可以通过一个对象的属性来迭代数据：

```html
<div id="app">
  <ul>
    <li v-for="value in object">
    {{ value }}
    </li>
  </ul>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    object: {
      name: '菜鸟教程',
      url: 'http://www.runoob.com',
      slogan: '学的不仅是技术，更是梦想！'
    }
  }
})
</script>
```

遍历对象处理嵌套。

```html
<div id="app">
  <ul>
    <li v-for="(value,key,index) in object">
        <p v-if="typeof value !='object'">{{value}}....{{ index }}</p>
        <p v-else>{{key}}....{{index}}</p>
        <ul v-if="typeof value == 'object'">
            <li v-for="(value, key, index) in value">
                {{key}}:{{value}}....{{ index }}
            </li>
        </ul>
    </li>
  </ul>
</div>
```

九九乘法

```html
<div id="app">
    <div v-for="n in 9">
        <b v-for="m in n">
            {{m}}*{{n}}={{m*n}}
        </b>
    </div>
</div>
```

# 计算属性

==computed==

```js
var vm = new Vue({
  el: '#app',
  data: {
    message: 'Runoob!'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  },
  methods: {
    reversedMessage2: function () {
      return this.message.split('').reverse().join('')
    }
  }
})
```

- `computed` vs `methods`
- 我们可以使用 methods 来替代 computed，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用 methods ，在重新渲染的时候，函数总会重新调用执行。
- 可以说使用 `computed` 性能会更好，但是如果你不希望缓存，你可以使用 `methods` 属性。
- `computed` 对象内的方法如果在初始化时绑定到元素上的事件会先执行一次这个方法 ，而 `methods` 内的方法则不会；

# 监听属性

通过 `watch` 来响应数据的变化。

千米与米之间的换算：

```js
<div id = "computed_props">
    千米 : <input type = "text" v-model = "kilometers">
    米 : <input type = "text" v-model = "meters">
</div>
<p id="info"></p>
<script type = "text/javascript">
    var vm = new Vue({
    el: '#computed_props',
    data: {
        kilometers : 0,
        meters:0
    },
    methods: {
    },
    computed :{
    },
    watch : {
        kilometers:function(val) {
            this.kilometers = val;
            this.meters = val * 1000;
        },
        meters : function (val) {
            this.kilometers = val/ 1000;
            this.meters = val;
        }
    }
    });
    // $watch 是一个实例方法
    vm.$watch('kilometers', function (newValue, oldValue) {
    // 这个回调将在 vm.kilometers 改变后调用
    document.getElementById ("info").innerHTML = "修改前值为: " + oldValue + "，修改后值为: " + newValue;
})
</script>
```

# 样式绑定

 `v-bind` 在处理 class 和 style 时， 专门增强了它。表达式的结果类型除了字符串之外，还可以是对象或数组。

## class 属性绑定

 ```html
<!--对象中传入更多属性-->
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>


<!--直接绑定数据里的一个对象-->
<div id="app">
  <div v-bind:class="classObject"></div>
</div>
```

```js
// 绑定返回对象的计算属性
new Vue({
  el: '#app',
  data: {
    isActive: true,
    error: {
      value: true,
      type: 'fatal'
    }
  },
  computed: {
    classObject: function () {
      return {
  base: true,
        active: this.isActive && !this.error.value,
        'text-danger': this.error.value && this.error.type === 'fatal',
      }
    }
  }
})
```

```html
<!--把一个数组传给 v-bind:class-->
<div v-bind:class="[activeClass, errorClass]"></div>

<!--使用三元表达式-->
<div v-bind:class="[errorClass ,isActive ? activeClass : '']"></div>
```

## style(内联样式)

```html
<!--v-bind:style 直接设置样式：-->
<div id="app">
    <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">菜鸟教程</div>
</div>

<!--直接绑定到一个(或多个)样式对象-->
<div id="app">
  <div v-bind:style="styleObject">菜鸟教程</div>
</div>
<script>
new Vue({
  el: '#app',
  data: {
    styleObject: {
      color: 'green',
      fontSize: '30px'
    }
  }
})
</script>
```

# 事件处理器 

事件监听可以使用 `v-on` 指令

```html
<div id="app">
  <button v-on:click="counter += 1">增加 1</button>
  <p>这个按钮被点击了 {{ counter }} 次。</p>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    counter: 0
  }
})
</script>
```

内联 JavaScript 语句

```html
<div id="app">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
 
<script>
new Vue({
  el: '#app',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
</script>
```

传入当前参数

```html
<button v-on:click="say('hi',$event)">say hi</button>

methods:{
  say:function(message,e){
     alert(message);
     console.log(e.currentTarget);
  }
}
```

## 事件修饰符

Vue.js 为 `v-on` 提供了事件修饰符来处理 DOM 事件细节，如：event.preventDefault() 或 event.stopPropagation()。

Vue.js通过由点(.)表示的指令后缀来调用修饰符。

```html
<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联  -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身（而不是子元素）触发时触发回调 -->
<div v-on:click.self="doThat">...</div>

<!-- click 事件只能点击一次，2.1.4版本新增 -->
<a v-on:click.once="doThis"></a>
```

## 按键修饰符

Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符：

```html
<!-- 只有在 keyCode 是 13 时调用 vm.submit() -->
<input v-on:keyup.13="submit">
<p><!-- Alt + C -->
<input @keyup.alt.67="clear">
<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```

# 表单

可以用 `v-model` 指令在表单控件元素上创建双向数据绑定。

v-model 会根据控件类型自动选取正确的方法来更新元素。

## 例子

全选与取消全选

```js
new Vue({
    el: '#app',
    data: {
        checked: false,
        checkedNames: [],
        checkedArr: ["Runoob", "Taobao", "Google"]
    },
    methods: {
        changeAllChecked: function() {
            if (this.checked) {
                this.checkedNames = this.checkedArr
            } else {
                this.checkedNames = []
            }
        }
    },
    watch: {
        "checkedNames": function() {
            if (this.checkedNames.length == this.checkedArr.length) {
                this.checked = true
            } else {
                this.checked = false
            }
        }
    }
})
```

## 修饰符

**.lazy**

在默认情况下， `v-model` 在 `input` 事件中同步输入框的值与数据，但你可以添加一个修饰符 lazy ，从而转变为在 change 事件中同步：

```html
<!-- 在 "change" 而不是 "input" 事件中更新 -->
<input v-model.lazy="msg" >
```

**.number**

如自动将用户的输入值转为 Number 类型（如果原值的转换结果为 NaN 则返回原值

```html
<input v-model.number="age" type="number">
```

**.trim**

自动过滤用户输入的首尾空格

```html
<input v-model.trim="msg">
```

[more info 表单输入绑定](https://cn.vuejs.org/v2/guide/forms.)

# 组件

组件是可复用的 Vue 实例，且带有一个名字：在这个例子中是 `<button-counter>`。我们可以在一个通过 new Vue 创建的 Vue 根实例中，把这个组件作为自定义元素来使用：

## 基本示例

所有实例都能用全局组件。

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

```js
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

## 组件的复用

每个组件都会各自独立维护它的 `count`,每用一次组件，就会有一个它的新实例被创建。