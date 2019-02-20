---
title: html5和css3基础知识
date: 2018-10-06 09:12:00
tags: html
categories: 开发者手册
toc: true
---
HTML5是HTML最新的修订版本，2014年10月由万维网联盟（W3C）完成标准制定。
<!-- more -->

# html5

新元素
新属性
完全支持 CSS3
Video 和 Audio
2D/3D 制图
本地存储
本地 SQL 数据
Web 应用

## 浏览器支持

---

- 将 HTML5 元素定义为块元素

- 兼容ie Shiv 解决方案

```html
<!--[if lt IE 9]>
<script src="http://cdn.static.runoob.com/libs/html5shiv/3.7/html5shiv.min.js"></script>
<![endif]-->
```

## 新增的结构标签

---
  
| 标签      |    描述 |
| :-------- | :--------| 
| **canvas新元素**  | |
| `<canvas>`  | 标签定义图形，比如图表和其他图像。该标签基于 JavaScript 的绘图 API|

## HTML5 Canvas

---

HTML5  `<canvas>`元素用于图形的绘制，通过脚本 (通常是JavaScript)来完成

`<canvas>` 标签只是图形容器，必须使用脚本来绘制图形。

简单实例：

```html
<canvas id="myCanvas" width="200" height="100" style="border:1px solid #d3d3d3;">
您的浏览器不支持 HTML5 canvas 标签。</canvas>

<script>
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
ctx.font="30px Arial";
ctx.strokeText("Hello World",10,50);
</script>
```

## 内联 SVG

---

**定义** 

- SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
- SVG 用于定义用于网络的基于矢量的图形
- SVG 使用 XML 格式定义图形
- SVG 图像在放大或改变尺寸的情况下其图形质量不会有损失
- SVG 是万维网联盟的标准

**SVG 与 Canvas两者间的区别**

SVG 是一种使用 XML 描述 2D 图形的语言。

Canvas 通过 JavaScript 来绘制 2D 图形。

SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。

在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。

Canvas 是逐像素进行渲染的。在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。

## 拖放（Drag 和 Drop）

---

拖放（Drag 和 drop）是 HTML5 标准的组成部分。

首先，为了使元素可拖动，把 draggable 属性设置为 true ：

```html
<img draggable="true">
```

## Geolocation（地理定位）

---

HTML5 Geolocation API 用于获得用户的地理位置。

鉴于该特性可能侵犯用户的隐私，除非用户同意，否则用户位置信息是不可用的。

demo 返回经纬度

```html
<body>
<p id="demo">点击按钮获取您当前坐标（可能需要比较长的时间获取）：</p>
<button onclick="getLocation()">点我</button>
<script>
var x=document.getElementById("demo");
function getLocation()
{
    if (navigator.geolocation)
    {
        navigator.geolocation.getCurrentPosition(showPosition);
    }
    else
    {
        x.innerHTML="该浏览器不支持获取地理位置。";
    }
}
​
function showPosition(position)
{
    x.innerHTML="纬度: " + position.coords.latitude + 
    "<br>经度: " + position.coords.longitude; 
}
</script>
</body>
```

## Video(视频)&&Audio(音频)

---

HTML5 `<video>` 和 `<audio>`   元素同样拥有方法、属性和事件。

`<video>` 和 `<audio>`元素的方法、属性和事件可以使用JavaScript进行控制.

其中的方法用于播放、暂停以及加载等。其中的属性（比如时长、音量等）可以被读取或设置。其中的 DOM 事件能够通知您，比方说，`<video>` 元素开始播放、已暂停，已停止，等等。

例中简单的方法，向我们演示了如何使用 `<video>` 元素，读取并设置属性，以及如何调用方法。
demo

```html
<body>
<div style="text-align:center">
  <button onclick="playPause()">播放/暂停</button>
  <button onclick="makeBig()">放大</button>
  <button onclick="makeSmall()">缩小</button>
  <button onclick="makeNormal()">普通</button>
  <br>
  <video id="video1" width="420">
    <source src="mov_bbb.mp4" type="video/mp4">
    <source src="mov_bbb.ogg" type="video/ogg">
    您的浏览器不支持 HTML5 video 标签。
  </video>
    <br> 
   <audio controls>
    <source src="horse.ogg" type="audio/ogg">
    <source src="horse.mp3" type="audio/mpeg">
  您的浏览器不支持 audio 元素。
  </audio>

</div>

<script>
var myVideo=document.getElementById("video1"); 

function playPause(){
if (myVideo.paused)
  myVideo.play();
else {
  myVideo.pause();
}

function makeBig(){
  myVideo.width=560;
}

function makeSmall(){
  myVideo.width=320;
}

 function makeNormal(){
  myVideo.width=420;
 }
</script>
```

## input类型

---

HTML5 拥有多个新的表单输入类型。这些新特性提供了更好的输入控制和验证。

并不是所有的主流浏览器都支持新的input类型，不过您已经可以在所有主流的浏览器中使用它们了。即使不被支持，仍然可以显示为常规的文本域。

`color` `date` `datetime` `datetime-local` `email` `month` `number` `range` `search` `tel` `time` `url` `week`

- input类型

  type=email,url,date,time,month,week,number,range,search,在移动端用的比较多

- 表单验证

- required => 验证表单是否为空，必须配合form表单来使用
- pattern => 自定义验证表单规则，匹配正则
- invalid => 验证失败的时候触发的事件

## 表单元素

---

`<datalist>` `<keygen>` `<output>` 

- datalist  `ie9`
  `<datalist>` 元素规定输入域的选项列表。

  `<datalist>` 属性规定 form 或 input 域应该拥有自动完成功能。当用户在自动完成域中开始输入时，浏览器应该在该域中显示填写的选项：`输入时模糊查询`

  使用 `<input>` 元素的列表属性与 `<datalist>` 元素绑定.

  demo

  ```html
  <form action="demo-form.php" method="get">
  <input list="browsers" name="browser">
  <datalist id="browsers">
    <option value="Internet Explorer">
    <option value="Firefox">
    <option value="Chrome">
    <option value="Opera">
    <option value="Safari">
  </datalist>
  <input type="submit">
  </form>
  ```

- keygen `ie`

`<keygen>` 元素的作用是提供一种验证用户的可靠方法。

`<keygen>`标签规定用于表单的密钥对生成器字段。

当提交表单时，会生成两个键，一个是私钥，一个公钥。

私钥（private key）存储于客户端，公钥（public key）则被发送到服务器。公钥可用于之后验证用户的客户端证书（client certificate）。

- output

 `<output>` 元素用于不同类型的输出，比如计算或脚本输出

## 表单属性

---

`<form>`新属性：`autocomplete` `novalidate`

`<input>`新属性：`autocomplete`、`autofocus`、`form`、`formaction`、`formenctype`、`formmethod`、
`formnovalidate`、`formtarget`、`height and width`、`list、min and max`、`multiple`、
`pattern (regexp)`、`placeholder`、`required`、`step`

## 语义元素 `ie9`

---

| **新的语义和结构元素**   |  |
| :----   | :---- |
| `<header>` `<footer>`      |   定义了文档的头部区域和页脚 |
| `<nav>`      |   定义导航链接的部分。 |
| `<section>`      |   定义文档中的节（section、区段）。 |
| `<article>`     |   标签定义独立的内容。|
| `<aside>`     |  定义页面的侧边栏内容。|
| `<details>`      |   用于描述文档或文档某个部分的细节 |
| `<mark>`      |   定义带有记号的文本。 |
| `<figure>`      |   标签定义 `<figure>` 元素的标题. |

## Web 存储 `ie8`

---

**什么是 HTML5 Web 存储?**

  使用HTML5可以在本地存储用户的浏览数据。

  早些时候,本地存储使用的是 cookie。但是Web 存储需要更加的安全与快速. 这些数据不会被保存在服务器上，但是这些数据只用于用户请求网站数据上.它也可以存储大量的数据，而不影响网站的性能.

  数据以 键/值 对存在, web网页的数据只允许该网页访问使用。

**localStorage 和 sessionStorage特性**

  `localStorage` - 用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去除。 
  `sessionStorage` - 用于临时保存同一窗口(或标签页)的数据，在关闭窗口或标签页之后将会删除这些数据。

- 常用API
- setItem(key,value)//设置存储内容
- getItem(key) //获取存储内容
- removeItem(key) //删除key值的存储内容
- claer() //清除所有存储内容
- key(n) //以索引值来获取存储内容

**localStorage**

```js
if(typeof(Storage)!=="undefined")
{
  localStorage.sitename="isyslh";
  document.getElementById("result").innerHTML="网站名：" + localStorage.sitename;
}
else
{
  document.getElementById("result").innerHTML="对不起，您的浏览器不支持 web 存储。";
}
```

**sessionStorage 对象**
sessionStorage 方法针对一个 session 进行数据存储。当用户关闭浏览器窗口后，数据会被删除。

## 应用程序缓存 `ie10`

使用 HTML5，通过创建 cache manifest 文件，可以轻松地创建 web 应用的离线版本。

什么是应用程序缓存（Application Cache）？

HTML5 引入了应用程序缓存，这意味着 web 应用可进行缓存，并可在没有因特网连接时进行访问。

应用程序缓存为应用带来三个优势：

离线浏览 - 用户可在应用离线时使用它们
速度 - 已缓存资源加载得更快
减少服务器负载 - 浏览器将只从服务器下载更新过或更改过的资源。

## WebSocket

WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

在 WebSocket API 中，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。

现在，很多网站为了实现推送技术，所用的技术都是 Ajax 轮询。轮询是在特定的的时间间隔（如每1秒），由浏览器对服务器发出HTTP请求，然后由服务器返回最新的数据给客户端的浏览器。这种传统的模式带来很明显的缺点，即浏览器需要不断的向服务器发出请求，然而HTTP请求可能包含较长的头部，其中真正有效的数据可能只是很小的一部分，显然这样会浪费很多的带宽等资源。

HTML5 定义的 WebSocket 协议，能更好的节省服务器资源和带宽，并且能够更实时地进行通讯。

![“websocket”](/images/20184/html51.png)  

浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，连接建立以后，客户端和服务器端就可以通过 TCP 连接直接交换数据。

当你获取 Web Socket 连接后，你可以通过 send() 方法来向服务器发送数据，并通过 onmessage 事件来接收服务器返回的数据。

以下 API 用于创建 WebSocket 对象。

# CSS3

选择器
盒模型
背景和边框
文字特效
2D/3D转换
动画
多列布局
用户界面

## 边框

- border-radius
- box-shadow
- border-image

## 圆角

- border-radius

## 背景

- background-image
- background-size
- background-origin
- background-clip

## 渐变（Gradients） `ie10`

- 线性渐变

  ```css
  #grad1 {
      height: 200px;
      background: -webkit-linear-gradient(red, blue); /* Safari 5.1 - 6.0 */
      background: -o-linear-gradient(red, blue); /* Opera 11.1 - 12.0 */
      background: -moz-linear-gradient(red, blue); /* Firefox 3.6 - 15 */
      background: linear-gradient(red, blue); /* 标准的语法（必须放在最后） */
  }
  ```

- 径向渐变

  ```css
  #grad1 {
      height: 150px;
      width: 200px;
      background: -webkit-radial-gradient(red, green, blue); /* Safari 5.1 - 6.0 */
      background: -o-radial-gradient(red, green, blue); /* Opera 11.6 - 12.0 */
      background: -moz-radial-gradient(red, green, blue); /* Firefox 3.6 - 15 */
      background: radial-gradient(red, green, blue); /* 标准的语法（必须放在最后） */
  }
  ```

更多：<http://www.runoob.com/css3/css3-gradients.html>

## 文本效果

`text-shadow`：文本阴影
水平阴影，垂直阴影，模糊的距离，以及阴影的颜色：
`box-shadow`：盒子阴影

```css
水平阴影    垂直阴影   模糊    阴影尺寸  颜色   外阴影转到内阴影 
0 4px 8px 0 rgba(0, 0, 0, 0.2)
```

`text-overflow`：文本溢出属性指定应向用户如何显示溢出内容

```css
text-overflow:ellipsis
```

`word-wrap`：换行

```css
p {word-wrap:break-word;}
```

`word-break`：单词拆分换行

```css
p.test1 {
    word-break: keep-all;
}
 
p.test2 {
    word-break: break-all;
}
```

hanging-punctuation：规定标点字符是否位于线框之外。
punctuation-trim：规定是否对标点字符进行修剪。
text-align-last：设置如何对齐最后一行或紧挨着强制换行符之前的行。
text-emphasis：向元素的文本应用重点标记以及重点标记的前景色。
text-justify：规定当 text-align 设置为 "justify" 时所使用的对齐方法。
text-outline：规定文本的轮廓。

## 字体

```css
@font-face
{
  font-family: myFirstFont;
  src: url('Sansation_Light.ttf')
    ,url('Sansation_Light.eot'); /* IE9 */
}
div
{
  font-family:myFirstFont;
}
```

## 2D转换

  `translate()`
  translate()方法，根据左(X轴)和顶部(Y轴)位置给定的参数，从当前元素位置`移动`。
  `rotate()`
  rotate()方法，在一个给定度数顺时针旋转的元素。负值是允许的，这样是元素逆时针`旋转`。
  `scale()`
  scale()方法，该元素`增加或减少的大小`，取决于宽度（X轴）和高度（Y轴）的参数：
  `skew()`
  包含两个参数值，分别表示X轴和Y轴`倾斜`的角度，如果第二个参数为空，则默认为0，参数为负表示向相反方向倾斜。
  `matrix()`
  matrix 方法有六个参数，包含旋转，缩放，移动（平移）和倾斜功能。

  ```css
  div#div2
  {
    transform:matrix(0.866,0.5,-0.5,0.866,0,0);
    -ms-transform:matrix(0.866,0.5,-0.5,0.866,0,0); /* IE 9 */
    -webkit-transform:matrix(0.866,0.5,-0.5,0.866,0,0); /* Safari and Chrome */
  }
  ```

## 3D 转换

rotateX()
rotateY()

## 过渡

要实现这一点，必须规定两项内容：

- 指定要添加效果的CSS属性
- 指定效果的持续时间

```css
div {
    width: 100px;
    height: 100px;
    background: red;
    -webkit-transition: width 2s, height 2s, -webkit-transform 2s; /* For Safari 3.1 to 6.0 */
    transition: width 2s, height 2s, transform 2s;
}

div:hover {
    width: 200px;
    height: 200px;
    -webkit-transform: rotate(180deg); /* Chrome, Safari, Opera */
    transform: rotate(180deg);
}
/* 简写 */
  transition:width 1s linear 2s;
  /* Safari */
  -webkit-transition:width 1s linear 2s;
```

## 动画

要创建CSS3动画，你将不得不了解@keyframes规则。

@keyframes规则是创建动画。 @keyframes规则内指定一个CSS样式和动画将逐步从目前的样式更改为新的样式。

当在 @keyframes 创建动画，把它绑定到一个选择器，否则动画不会有任何效果。

指定至少这两个CSS3的动画属性绑定向一个选择器：

- 规定动画的名称
- 规定动画的时长

```css
/* 简写 */
div
{
    width:100px;
    height:100px;
    background:red;
    position:relative;
   /* 规定 @keyframes 动画的名称。
      完成一个周期所花费的秒或毫秒。默认是 0。
      规定动画的速度曲线。默认是 "ease"。
      规定动画何时开始。默认是 0。
      规定动画被播放的次数。默认是 1。
      规定动画是否在下一周期逆向地播放。默认是 "normal"。 */
    animation:myfirst 5s linear 2s infinite alternate;
    /* Firefox: */
    -moz-animation:myfirst 5s linear 2s infinite alternate;
    /* Safari and Chrome: */
    -webkit-animation:myfirst 5s linear 2s infinite alternate;
    /* Opera: */
    -o-animation:myfirst 5s linear 2s infinite alternate;
}
​
@keyframes myfirst
{
    0%   {background:red; left:0px; top:0px;}
    25%  {background:yellow; left:200px; top:0px;}
    50%  {background:blue; left:200px; top:200px;}
    75%  {background:green; left:0px; top:200px;}
    100% {background:red; left:0px; top:0px;}
}
​
@-moz-keyframes myfirst /* Firefox */
{
    0%   {background:red; left:0px; top:0px;}
    25%  {background:yellow; left:200px; top:0px;}
    50%  {background:blue; left:200px; top:200px;}
    75%  {background:green; left:0px; top:200px;}
    100% {background:red; left:0px; top:0px;}
}
​
@-webkit-keyframes myfirst /* Safari and Chrome */
{
    0%   {background:red; left:0px; top:0px;}
    25%  {background:yellow; left:200px; top:0px;}
    50%  {background:blue; left:200px; top:200px;}
    75%  {background:green; left:0px; top:200px;}
    100% {background:red; left:0px; top:0px;}
}
​
@-o-keyframes myfirst /* Opera */
{
    0%   {background:red; left:0px; top:0px;}
    25%  {background:yellow; left:200px; top:0px;}
    50%  {background:blue; left:200px; top:200px;}
    75%  {background:green; left:0px; top:200px;}
    100% {background:red; left:0px; top:0px;}
}
```

## 多列

可以将文本内容设计成像报纸一样的多列布局


属性：描述
column-count：指定元素应该被分割的列数。
column-fill：指定如何填充列
column-gap：指定列与列之间的间隙
column-rule：所有 column-rule-* 属性的简写
column-rule-color：指定两列间边框的颜色
column-rule-style：指定两列间边框的样式
column-rule-width：指定两列间边框的厚度
column-span：指定元素要跨越多少列
column-width：指定列的宽度
columns：设置 column-width 和 column-count 的简写

## 用户界面

- resize

  指定一个元素是否应该由用户去调整大小。

- box-sizing

  以确切的方式定义适应某个区域的具体内容。

- outline-offset

  对轮廓进行偏移，并在超出边框边缘的位置绘制轮廓。

  - 1、轮廓不占用空间

  - 2、轮廓可能是非矩形  

## 框大小

- box-sizing 属性

  如果在元素上设置了 box-sizing: border-box; 则 padding(内边距) 和 border(边框) 也包含在 width 和 height 中:

  ``` css
  .div1 {
    width: 300px;
    height: 100px;
    border: 1px solid blue;
    box-sizing: border-box;
  }

  .div2 {
      width: 300px;
      height: 100px;
      padding: 50px;
      border: 1px solid red;
      box-sizing: border-box;
  }
  ```

## 弹性盒子(Flex Box)

## 多媒体查询

CSS3 的多媒体查询继承了 CSS2 多媒体类型的所有思想： 取代了查找设备的类型，CSS3 根据设置自适应显示。

媒体查询可用于检测很多事情，例如：

- viewport(视窗) 的宽度与高度
- 设备的宽度与高度
- 朝向 (智能手机横屏，竖屏) 。
- 分辨率

多媒体查询由多种媒体组成，可以包含一个或多个表达式，表达式根据条件是否成立返回 true 或 false。

```css
@media not|only mediatype and (expressions) {
    CSS 代码...;
}

not: not是用来排除掉某些特定的设备的，比如 @media not print（非打印设备）。
only: 用来定某种特别的媒体类型。对于支持Media Queries的移动设备来说，如果存在only关键字，移动设备的Web浏览器会忽略only关键字并直接根据后面的表达式应用样式文件。对于不支持Media Queries的设备但能够读取Media Type类型的Web浏览器，遇到only关键字时会忽略这个样式文件。
all: 所有设备，这个应该经常看到。

mediatype:
all:用于所有多媒体类型设备
print:用于打印机
screen:用于电脑屏幕，平板，智能手机等。
speech:用于屏幕阅读器
```

如果指定的多媒体类型匹配设备类型则查询结果返回 true，文档会在匹配的设备上显示指定样式效果。

除非你使用了 not 或 only 操作符，否则所有的样式会适应在所有设备上显示效果。

你也可以在不同的媒体上使用不同的样式文件：

```html
<link rel="stylesheet" media="mediatype and|not|only (expressions)" href="print.css">
```

demo

```css
/* 屏幕可视窗口尺寸大于 480 像素的设备上修改背景颜色: */
body {
    background-color: pink;
}

@media screen and (min-width: 480px) {
    body {
        background-color: lightgreen;
    }
}
```

