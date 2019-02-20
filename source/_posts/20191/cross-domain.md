---
title: js跨域相关问题
toc: true
date: 2019-01-30 11:15:09
tags:
categories: javascript
---

什么是跨域？为什么跨域？如何解决...

<!-- more -->

# 一、什么是跨域

跨域是指一个域下的文档或脚本试图去请求另一个域下的资源，这里跨域是广义的。

广义的跨域：

- 1、资源跳转： A链接、重定向、表单提交
- 2、资源嵌入： `<link>`、`<script>`、`<img>`、`<frame>`等dom标签，还有样式中background:url()、@font-face()等文件外链
- 3、脚本请求： js发起的ajax请求、dom和js对象的跨域操作等

我们通常说的跨域是狭义的，是由浏览器同源策略限制的一类请求场景

>`同源策略/SOP（Same origin policy）`是一种约定，由Netscape公司1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到XSS、CSFR等攻击。所谓同源是指"协议+域名+端口"三者相同，即便两个不同的域名指向同一个ip地址，也非同源。

同源策略限制以下几种行为：

- 1、 Cookie、LocalStorage 和 IndexDB 无法读取
- 2、 DOM 和 Js对象无法获得
- 3、 AJAX 请求不能发送

**常见跨域场景**：

```txt
URL                                      说明                    是否允许通信
http://www.domain.com/a.js
http://www.domain.com/b.js         同一域名，不同文件或路径           允许
http://www.domain.com/lab/c.js

http://www.domain.com:8000/a.js
http://www.domain.com/b.js         同一域名，不同端口                不允许

http://www.domain.com/a.js
https://www.domain.com/b.js        同一域名，不同协议                不允许

http://www.domain.com/a.js
http://192.168.4.12/b.js           域名和域名对应相同ip              不允许

http://www.domain.com/a.js
http://x.domain.com/b.js           主域相同，子域不同                不允许
http://domain.com/c.js

http://www.domain1.com/a.js
http://www.domain2.com/b.js        不同域名                         不允许
```

# 二、为什么浏览器要限制跨域访问呢

原因就是`安全问题`：如果一个网页可以随意地访问另外一个网站的资源，那么就有可能在客户完全不知情的情况下出现安全问题。比如下面的操作就有安全问题：

- 用户访问www.mybank.com ，登陆并进行网银操作，这时cookie啥的都生成并存放在浏览器
- 用户突然想起件事，并迷迷糊糊地访问了一个邪恶的网站 www.xiee.com
- 这时该网站就可以在它的页面中，拿到银行的cookie，比如用户名，登陆token等，然后发起对www.mybank.com 的操作。
- 如果这时浏览器不予限制，并且银行也没有做响应的安全处理的话，那么用户的信息有可能就这么泄露了。

# 三、跨域解决方案

## 1.jsonp

1.）可跨域的HTML标签：

所有具有src属性的HTML标签都是可以跨域的，包括`<script>``<img>``<iframe>`,所以我们通常会把一些图片资源放到第三方服务器上，然后可以通过`<img>`标签的src属性引用。例如：

```js
var img = new Image()
img.src = 'http://some/picture'      // 发送http请求
```

2.）原生实现：

```js
 <script>
    var script = document.createElement('script');
    script.type = 'text/javascript';

    // 传参并指定回调执行函数为onBack
    script.src = 'http://www.domain2.com:8080/login?user=admin&callback=onBack';
    document.head.appendChild(script);

    // 回调执行函数
    function onBack(res) {
        alert(JSON.stringify(res));
    }
 </script>
```

服务端返回如下（返回时即执行全局函数）：

```js
onBack({"status": true, "user": "admin"})
```

3.）jquery ajax：

```js
$.ajax({
    url: 'http://www.domain2.com:8080/login',
    type: 'get',
    dataType: 'jsonp',  // 请求方式为jsonp
    jsonpCallback: "onBack",    // 自定义回调函数名
    data: {}
});
```

4.）vue.js：

```js
this.$http.jsonp('http://www.domain2.com:8080/login', {
    params: {},
    jsonp: 'onBack'
}).then((res) => {
    console.log(res);
})
```

后端node.js代码示例：

```js
var querystring = require('querystring');
var http = require('http');
var server = http.createServer();

server.on('request', function(req, res) {
    var params = qs.parse(req.url.split('?')[1]);
    var fn = params.callback;

    // jsonp返回设置
    res.writeHead(200, { 'Content-Type': 'text/javascript' });
    res.write(fn + '(' + JSON.stringify(params) + ')');

    res.end();
});

server.listen('8080');
console.log('Server is running at port 8080...');
```

jsonp缺点：只能实现get一种请求。






