---
title: "H5 Canvas前端绘图库 Fabric 简单应用"
categories: [ "前端技术" ]
tags: [  ]
draft: false
slug: "fabricjs"
date: "2020-12-09 12:46:00"
---

圣诞节又快到了，又到了全民给微信头像加圣诞帽的时候了。
撸了一个纯前端实现的加圣诞帽的页面，[点击前往][1]

## JS引用

```js
<script src="https://cdn.bootcdn.net/ajax/libs/fabric.js/4.2.0/fabric.min.js"></script>
```
直接用 bootcdn 引用 fabric.js 最新版

## 构建 fabric 对象
```html
<div class="body" id="uploadContainer">
<img id="export" alt="圣诞快乐" src="" />
<canvas id="cvs"></canvas>

<div style="display: none">
  <img id="img" src="" alt="" />
  <img class="hide" id="hat0" src="./hat0.png" />
  <!-- 此处省略其余帽子图像-->
</div>
```

部分DOM内容，只保留关键组件

```js
var screenWidth = window.screen.width < 500 ? window.screen.width : 300 // 计算是最终输出大小（适配移动端）

canvasFabric = new fabric.Canvas('cvs', {
  width: screenWidth ,
  height: screenWidth ,
  backgroundImage: new fabric.Image(img, {
    scaleX: screenWidth / img.width, // 强制缩放至正方形
    scaleY: screenWidth / img.height,
  })
```

要对 canvas进行操作，首先要构建一个对象
```js
hatInstance = new fabric.Image(hatImage, { // 以一个img DOM创建fabric图像对象
  top: 40,
  left: 50,
  scaleX: 100 / hatImage.width,
  scaleY: 100 / hatImage.height,
  cornerColor: '#0b3a42',
  cornerStrokeColor: '#fff',
  cornerStyle: 'circle',
  transparentCorners: false,
  rotatingPointOffset: 30,
})
hatInstance.setControlVisible('bl', false) // 默认的对象都会有控制大小的手柄，此处关闭几个
hatInstance.setControlVisible('tr', false)
hatInstance.setControlVisible('tl', false)
hatInstance.setControlVisible('mr', false)
hatInstance.setControlVisible('mt', false)
canvasFabric.add(hatInstance) // 添加实例到画布中
```
随后就是在头像图上绘制圣诞帽
创建一个 Image 对象，并将其添加到画布中。

```js
exportImage.src = canvasFabric.toDataURL({ width: screenWidth, height: screenWidth })
```
最终将画布内容输出到img DOM中，移动端长按保存，桌面端右键保存。

## 源码
[button color="light" icon="" url="https://cdn.rhyland.cn/typecho/2020/12/09/christmas_hat.zip" type="round"]点击下载[/button]


  [1]: https://blog.xingez.me/christmas_hat