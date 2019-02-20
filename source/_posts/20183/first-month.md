---
title: 遇到的问题
data: 2018-08-20 15:37:57
tags: helloworld
categories: 开发者手册
toc: true
---

记录一下项目中相关的的问题

<!-- more -->

## 用CANVAS绘制水印

```js
watermark: function (str, currentDivId) {

  let watermark = {}

  let setWatermark = (str) => {

      let id = 'addWarteMark';
      if (document.getElementById(id) !== null) {
          // document.body.removeChild(document.getElementById(id))
          document.getElementById(currentDivId).removeChild(document.getElementById(id))
      }
      let can = document.createElement('canvas')
      can.width = 150
      can.height = 120

      let cans = can.getContext('2d')
      cans.rotate(45 * Math.PI / 180)
      cans.font = '20px Vedana'
      cans.fillStyle = 'rgba(222, 222, 222, 1.0)'
      cans.textAlign = 'left'
      cans.textBaseline = 'Middle'
      cans.fillText(str, can.width / 4, can.height / 6)

      let div = document.createElement('div')
      div.id = id
      // div.style.pointerEvents = 'none'
      // div.style.top = '70px'
      // div.style.left = '0px'
      // div.style.position = 'fixed'
      div.style.pointerEvents = 'none'
      div.style.top = '0px'
      div.style.left = '0px'
      div.style.position = 'absolute'
      // div.style.zIndex = '100000'
      //   div.style.zIndex = '0'
      // div.style.width = document.documentElement.clientWidth - 100 + 'px'
      // div.style.height = document.documentElement.clientHeight - 100 + 'px'
      div.style.width = document.documentElement.scrollWidth + 'px'
      div.style.height = document.documentElement.scrollHeight + 'px'
      // div.style.width = document.documentElement.scrollWidth/70 + 'rem'
      // div.style.height = document.documentElement.scrollHeight/70 + 'rem'
      div.style.background = 'url(' + can.toDataURL('image/png') + ') left top repeat';
      document.getElementById(currentDivId).appendChild(div)
      // document.body.appendChild(div)
      return id
  }

  // 该方法只允许调用一次
  let id = setWatermark(str)
  setInterval(() => {
      if (document.getElementById(id) === null) {
        id = setWatermark(str)
      }
  }, 500)
  window.onresize = () => {
      setWatermark(str, currentDivId)
  }
 },

```

## HTML中数字和字母不换行显示

在HTML中标签中的数字和字母默认是不换行的，如果要将他们换行，在CSS中添加”word-wrap: break-word;” 即可解决 

语法：```word-wrap: normal|break-word;```

默认normal : 单词默认是不能断开的，长单词就会溢出
break-word : 会首先起一个新行来放置长单词，新的行还是放不下这个长单词则会对长单词进行强制断句。

如果需要，词内换行使用（word-break）即可

语法：word-break:break-all;不会把长单词放在一个新行里，当这一行放不下的时候就直接强制断句了。

## vue表单enter键刷新页面

v-on:submit.prevent

## inline-block元素设置overflow:hidden属性导致相邻行内元素向下偏移

### 2. 解决方法

常用的解决方法是为上述inline-block元素添加vertical-align: bottom。你可以在上面的例子中在线测试下

### 3. 问题原因

W3的规范对此行为有明确规定：The baseline of an 'inline-block' is the baseline of its last line box in the normal flow, unless it has either no in-flow line boxes or if its 'overflow' property has a computed value other than 'visible', in which case the baseline is the bottom margin edge.我们从规范中可以知道当一个inline-block元素被设置overflow非visible属性值后，其baseline将被强制修改为元素下外边沿。我们知道默认情况下，baseline为字符x的底线位置且vertical-align属性值为baseline。此外由于div包含块中只有行内级别的元素，所以将生成一个IFC（行内格式化上下文）来规定其中元素的渲染规则。按照IFC布局规则，垂直方向上对齐遵照vertical-align属性，那么p元素两边的2个匿名line box将被迫向下移动一个偏移值来和p元素对齐。那么可能有人要进一步追问了，为什么W3要做如此奇怪的规定呢？这是因为overflow被设置为非visible值，将使得该inline-block元素中的last line box的渲染处于不确定状态（浏览器可能渲染也可能不渲染），这让保持默认规则的baseline也处于不确定状态，那么索性就规定以确定的下外边沿来作为baseline。

## 1，传参

- 浏览器缓存传参，可以使用 `UNESCAPE（）` 对 `ESCAPE（）` 编码的字符串进行解码。

```javascript
if (!window.localStorage) {
    console.log("该浏览器不支持localStorage")
    } else {
    window.localStorage.setItem("bfdwListInfo", JSON.stringify(GLOBAL.bfdwList));
    //  window.location.href = CONTEXTPATH + "/publicity/hhmd/red-black-detail.do?type=" + GLOBAL.type + "&navId=" + GLOBAL.navId + "&columnName=" + escape(GLOBAL.columnName);
}
//接收
bfdwListInfo: JSON.parse(window.localStorage.getItem("bfdwListInfo")),
```

- 新打开窗口传参（预览）

```js
 var info = {
    flag: 'report',
    content: ue.getContent(), // 获取编辑器的内容
    fbsj: $("#publishTime").val() //发布时间
};
// 序列化
$('input[name="preView"]').val(JSON.stringify(info));
var map = { '合肥': 'hefei'};
cityFlag = map[$('#cityName').val()];
window.open(CONTEXTPATH + '/preview/preview-' + ('ds') + '.do?cityFlag=' + cityFlag);

// 反序列化
var info = JSON.parse(window.opener.$('input[name="preView"]').val());
$('#credit').tmpl(info).appendTo('#creditDetail');

<script id="credit" type="text/x-jquery-tmpl">
    <h2 class="credit-title" title="\${title}">\${title}</h2>
        <h3 class="credit-time">
            <span class="ml20 ell" title="\${fbsj}">（更新时间：\${fbsj}）</span>
        </h3>
    <div class="credit-content">{{html content}}</div>
 </script>
 ```

## 2、统一提示

```js
 common.Tip.error(message.get(msg) || message.get('LOAD_ERROR'));
common.Tip.warning("请选择正确的颁发单位!");
utils.systemTip("请选择正确的颁发单位!", "error");
```

## 3、jquery.tmpl 和fly.template

```js
//js
$("#menuTmpl").tmpl(menu.rows).appendTo("#menu");//渲染列表
//html
{{if bfdwList.length}}
    {{each(i,deptName) bfdwList}}
        \${deptName}，
    {{/each}}
{{else}}
--
{{/if}}
```

## 5、传参问题

```base
后台接收对象，前台传键值对并 json.stringify
后台接收数组，前台传逗号分隔的字符串
arr.join(".")
```

>序列化：

```js
// 序列化

//localStorage.setItem('info', JSON.stringify(info));

$('input[name="preView"]').val(JSON.stringify(info));

var map = {

    '滁州': 'chuzhou',

    '合肥': 'hefei'

};

cityFlag = map[$('#cityName').val()];

window.open(CONTEXTPATH + '/preview/preview-' + (cityFlag || 'ds') + '.do?cityFlag=' + cityFlag);
```

> 接收：

```js
           // 反序列化

            var info = JSON.parse(window.opener.$('input[name="preView"]').val());

            $('#credit').tmpl(info).appendTo('#creditDetail');

            $('#editOne').tmpl(info).appendTo('#editList');
```

## 6、有关弹窗操作

- 关闭弹窗,刷新页面

```js
// 关闭弹窗
common.closeDialog('detailDialog');  
parent.fly.dialog.list.detailDialog.destroy();//不能用同一js

//刷新页面
window.parent.vm.notDs.page(1);
window.parent.location.reload();
```

- 调用父弹窗函数
```
window.parent.getDataSourceDatabyChild();

window.parent.getDataSourceDatabyChild = function () {
    var result = $('#xtylyNotForm').flyForm().data('flyForm').filter();
    vm.notDs.filter(result);
};
```
## 7、自定义校验
```
var rule = {
	shyj: {
		required: true,
		max: 2048,
		check: function() { return true }
	},
};
var $form = $('#submitForm').flyForm({
	valid: rule
});
var data = $form.data('flyForm').data();

//提示
$('#checked0').flyTooltip({
	content: '请选择审核状态'
});
return false;
```
## 8、下拉框属性

```
data-text-field="deptName"
data-value-field="deptCode"
data-filter="contains" //模糊查询
```

## 9、操作动态数据
```
for (var i = 0; i < arr.length; i++) {
	$('.issuetbody').find("[data-id="+ arr[i].deptCode+"]").attr('checked',true)
}
```

## 10、JQuery选择器
```
.find('.search-form form:not(".hide")')
$("input[name='newsletter']")   
```

## 11、数组操作
```
.json()

// 原来的数组  
var array = ["one", "two", "four"];  
// splice(position, numberOfItemsToRemove, item)  
// 拼接函数(索引位置, 要删除元素的数量, 元素)  
array.splice(2, 0, "three");  
  
array;  // 现在数组是这个样子 ["one", "two", "three", "four"] 
```

## 12、字符串操作
```
//字符串替换
var reg = new RegExp( '-' , "g" )
$('.assess-bgDate').html(GLOBAL.startdate.substr(0,10).replace(reg, "."));

//判断以...开头
if (/^http:/.test(url)) {                                    
	$('#iframeContent').attr('src', url);
}
```

## 13、