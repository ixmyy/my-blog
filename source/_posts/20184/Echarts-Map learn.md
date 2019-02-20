---
title: Echarts Map学习
date: 2018-11-20 15:37:57
tags: Echarts
categories: javascript
---
做项目的时候用到了百度地图，本文记录一些可以用到的地方

<!-- more -->


## 1、自定义地图样式  

可以通过[百度地图个性在线编辑器](https://developer.baidu.com/map/custom/)来自定义样式，选择好自己想要的风格配色之后 点击右下角的查看json 把json应用到setMapStyle的参数中就可以了
![页面截图](/images/map-json.png)

## 2、结合Echarts Scatter,可以有更多的交互
![效果图](/images/map-dian.png)


## 3、引入百度地图，可使用百度地图的相关控件
可以通过 var bmap = hotMap.getModel().getComponent(‘bmap‘).getBMap(); 这种方式获取地图对象，然后就可以随心所欲的操作了，百度地图相关操作方法可以参考[百度地图API](http://lbsyun.baidu.com/index.php?title=jspopular)。

## 4、其他
代码位置在
@(credit-standar2.1\credit-standard2.1-hefei\credit-score\src\main\webapp\WEB-INF\views\gis-map)