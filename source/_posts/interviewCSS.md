---
title: CSS面试题
# single_column: true
---

# CSS 高频面试题

## 01. 盒模型宽度计算

在如下代码中类名为 box 的 div，它的 offsetWidth 是多少?

```html
<style>
  #box {
    width: 100px;
    padding: 10px;
    border: 1px solid skyblue;
    margin: 10px;
  }
</style>
<div id="box"></div>
<script>
  console.log(document.getElementById("box").offsetWidth);
</script>
```

---

offsetWidth = 内容宽度 + 内边距 + 边框

box 的内容宽度为 100、左右内边距各为 10、左右边框各为 1、所以盒子的 offsetWidth 为 122。

通过 box-sizing 可以设置到底要如何计算一个元素的总宽度和总高度。

它的默认值为 content-box，即指定 width 属性值为内容宽度，盒子的实际宽度为内容宽度 + 内边距 + 边框。

它的值也可以是 border-box，即指定 width 属性值为盒子的总宽度，在设置了内边距和边框的情况下会挤压盒子内容的宽度。

## 02. 外边距负值

外边距在四个方向上设置负值会产生什么效果。

margin-top 设置正值元素向下移动、设置负值向上移动。

margin-left 设置正值元素向右移动、设置负值向左移动。

margin-right 设置正值右侧元素向右移动、设置负值右侧元素向左移动。

margin-bottom 设置正值下方元素向下移动，设置负值下方元素向上移动。

```html
<style>
  .box1 {
    width: 100px;
    height: 100px;
    background: skyblue;
  }
  .box2 {
    width: 100px;
    height: 100px;
    background: purple;
  }
</style>
<div class="box1"></div>
<div class="box2"></div>
```

## 03. 外边距重叠

① 什么是外边距重叠

两个块级元素的上外边距和下外边距可能会合并为一个外边距，这种现象被称之为外边距重叠。

外边距重叠只发生在垂直方向，水平方向不会重叠。浮动的元素和绝对定位的元素的外边距不会折叠。

② 外边距重叠的计算方式

(1) 如果两者都是正数，取最大值为最终的外边距值。

```html
<style>
  .box1 {
    width: 100px;
    height: 100px;
    background: skyblue;
    margin-bottom: 20px;
  }
  .box2 {
    width: 100px;
    height: 100px;
    background: purple;
    margin-top: 30px;
  }
</style>
<div class="box1"></div>
<div class="box2"></div>
```

(2) 如果两者一正一负，使用正值减去负值的绝对值，得到的结果为最终的外边距值。

```html
<style>
  .box1 {
    width: 100px;
    height: 100px;
    background: skyblue;
    margin-bottom: 20px;
  }
  .box2 {
    width: 100px;
    height: 100px;
    background: purple;
    margin-top: -10px;
  }
</style>
<div class="box1"></div>
<div class="box2"></div>
```

(3) 如果两者都是负值，使用 0 减去两个中值中绝对值大的那个，得到的结果为最终的外边距值

```html
<style>
  .box1 {
    width: 100px;
    height: 100px;
    background: skyblue;
    margin-bottom: -20px;
  }
  .box2 {
    width: 100px;
    height: 100px;
    background: purple;
    margin-top: -10px;
  }
</style>
```

③ 如何解决外边距重叠

(1) 不要同时为两个相邻的块级元素同时设置垂直方向上的边距(推荐)

(2) 为下层元素设置浮动或定位(绝对定位、固定定位)或 inline-block

## 04. 外边距塌陷

① 什么是外边距塌陷

两个嵌套关系的(一般为父子关系)块元素，当父元素有上外边距子元素也有上外边距时，两个上外边距会合成一个上外边距。

```html
<style>
  body {
    margin: 0;
  }
  .box1 {
    width: 100px;
    background: skyblue;
    margin-top: 10px;
  }
  .box2 {
    width: 100px;
    height: 100px;
    background: purple;
    margin-top: 10px;
  }
</style>
<div class="box1">
  <div class="box2"></div>
</div>
```

② 如何解决外边距塌陷

(1) 为父元素设置 overflow: auto/hidden

(2) 为父元素设置浮动

(3) 为父元素设置 display: inline-block

(4) 为父元素设置 border: 1px solid transparent

(5) 为父元素设置 padding: 1px

## 05. 清除浮动

子元素浮动后父元素高度撑不开的问题如何解决

```html
<style>
  .parent {
    width: 100%;
    background: skyblue;
  }
  .item {
    width: 200px;
    height: 100px;
    background: purple;
    float: left;
  }
</style>
<div class="parent">
  <div class="item"></div>
  <div class="item"></div>
</div>
```

① 为父级元素添加 overflow: hidden 或 overflow: auto

```css
.parent {
  overflow: hidden;
}
```

② 通过伪类元素解决浮动父级元素高度问题

```css
.clearfix::after {
  content: " ";
  visibility: hidden;
  display: block;
  height: 0;
  clear: both;
}
```

```html
<div class="parent clearfix"></div>
```

## 06. 理解 BFC

① 什么是 BFC

BFC 全称 Block Formatting Context， 意为块级格式化上下文。

BFC 其实就是指一块能够独立渲染的区域，并对这块区域内部的块级元素如何布局进行了规定。

比如块级元素默认在盒子的左上角进行渲染、块级盒子独占一行垂直排列、块级盒子之间的间距由 margin 设置。

由于通过 BFC 产生了一块独立渲染的区域，所以该区域内的元素无论怎样布局都不会影响到区域以外的元素。

只有块级元素可以具备 BFC 特性。

通过理解和 BFC 相关的知识能够对布局过程中产生问题进行快速解决，比如外边距重叠、外边距塌陷、浮动父级高度无法撑开等问题。

---

② 如何使元素具有 BFC 特性

(1) 根元素(HTML)

(2) 浮动之后的元素(float 属性的值不为 none)

(3) 绝对定位和固定定位之后的元素

(4) 行内块元素

(5) 表格单元格(display: table-cell)、表格标题(display: table-caption)

(6) overflow 属性值不为 visible 的块元素

(7) 弹性盒元素(display: flex)

(8) 网格元素 (display: grid)

---

③ BFC 特性

在同一个 BFC 中相邻的两个块级元素垂直方向上的外边距会被折叠

BFC 盒子的不会与浮动盒子产生交集而是紧贴着浮动元素的边缘

计算 BFC 盒子的高度时也会检测浮动盒子的高度

## 07. 圣杯布局

① 什么是圣杯布局

圣杯布局是指三栏布局，左右两栏宽度固定、中间宽度自适应。

在圣杯布局中要求中间一栏最先加载出来。

<img src="./images/01.webp" align="left" width="60%"/>

② 实现圣杯布局

```html
<style>
  body {
    margin: 0;
  }
  .container {
    padding: 0 150px 0 200px;
  }
  .column {
    float: left;
  }
  .left {
    width: 200px;
    background: skyblue;
    margin-left: -100%;
    position: relative;
    left: -200px;
  }
  .center {
    width: 100%;
    background: purple;
  }
  .right {
    width: 150px;
    background: palegreen;
    margin-left: -150px;
    position: relative;
    left: 150px;
  }
  .clearfix::after {
    content: " ";
    visibility: hidden;
    display: block;
    height: 0;
    clear: both;
  }
</style>
<div class="container clearfix">
  <div class="center column">center</div>
  <div class="left column">left</div>
  <div class="right column">right</div>
</div>
```

## 08. 双飞翼布局

① 什么是双飞翼布局

双飞翼布局和圣杯布局一样都是要实现三栏布局，两侧栏宽度固定，中间栏宽度自适应。

② 实现双飞翼布局

```html
<style>
  body {
    margin: 0;
  }
  .main-wrap {
    width: 100%;
    background: purple;
  }
  .main {
    margin: 0 150px 0 200px;
  }
  .column {
    float: left;
  }
  .left {
    width: 200px;
    background: skyblue;
    margin-left: -100%;
  }
  .center {
    width: 100%;
  }
  .right {
    width: 150px;
    background: palegreen;
    margin-left: -150px;
  }
  .clearfix::after {
    content: " ";
    visibility: hidden;
    display: block;
    height: 0;
    clear: both;
  }
</style>
<div class="container clearfix">
  <div class="main-wrap column">
    <div class="main">center</div>
  </div>
  <div class="left column">left</div>
  <div class="right column">right</div>
</div>
```

## 09. 弹性盒布局

```html
<style>
  .container {
    width: 1200px;
    height: 500px;
    border: 2px solid #000;
  }
  .item {
    width: 200px;
    height: 100px;
  }
  .item_1 {
    background: skyblue;
  }
  .item_2 {
    background: purple;
  }
  .item_3 {
    background: orangered;
  }
</style>

<div class="container">
  <div class="item item_1"></div>
  <div class="item item_2"></div>
  <div class="item item_3"></div>
</div>
```

<img src="./images/02.png" />

```css
.container {
  /* 
    通过 display: flex 将 container 设置为弹性容器
    container 的直接一级子元素自动成为弹性盒子
    弹性盒子默认按照主轴方向进行排列, 而主轴方向默认又是水平的, 从左到右
    所以当 container 被设置为弹性容器以后, 弹性盒子自动在水平方向上从左到右排列
  */
  display: flex;
}
```

<img src="./images/03.png" />

```css
.container {
  /*
    通过 justify-content 可以设置弹性盒子在主轴方向上的对应方式
    flex-start: 左对齐
    flex-end: 右对齐
    center: 居中对齐
    space-between: 盒子与盒子之间平均分配空间(不包含第一个盒子的左边和最后一个盒子的右边)
    space-evenly: 在盒子与盒子之间凭据分配间距(包含第一个盒子的左边和最后一个盒子的右边)
    space-around: 在盒子的两边凭据分配间距
  */
  justify-content: space-around;
}
```

<img src="./images/04.png" />

<img src="./images/05.png" />

<img src="./images/06.png" />

<img src="./images/07.png" />

<img src="./images/08.png" />

```css
.container {
  /* 
    通过 align-items 可以设置弹性盒子在侧轴方向上的对齐方式
    flex-start: 顶对齐
    flex-end: 底对齐
    center: 垂直居中对齐
  */
  align-items: center;
}
```

<img src="./images/09.png" />

<img src="./images/10.png" />

<img src="./images/11.png" />

```css
.container {
  align-items: center;
}
.item_3 {
  /* 通过 align-self 属性可以单独设置某一个弹性盒子的侧轴方向上的对齐方式 */
  align-self: flex-end;
}
```

<img src="./images/12.png" />

```css
.container {
  /* 设置弹性盒子在主轴方向上左对齐 */
  justify-content: flex-start;
}
.item_3 {
  /* 通过 margin auto 可以单独设置某一个弹性盒子在主轴方向上位置 */
  /* 将第三个盒子推向最右侧 */
  margin-left: auto;
}
```

<img src="./images/13.png" />

```css
.item_2 {
  /* 当父级有剩余空间时, 通过扩展当前元素的宽度占据所有剩余空间 */
  flex-grow: 1;
}
```

<img src="./images/14.png" />

```css
/* 将父级剩余空间划分为4分, 通过扩展弹性盒子占据剩余空间, item_1, item_3 占四分之一, item_2 占四分之二*/
.item_1 {
  flex-grow: 1;
}
.item_2 {
  flex-grow: 2;
}
.item_3 {
  flex-grow: 1;
}
```

<img src="./images/15.png" />

```css
.container {
  width: 1200px;
}
.item {
  width: 500px;
}
/* 
  当父级宽度不足以放置所有弹性盒子时, 所有弹性盒子的宽度默认会被缩减, 直到父级可以放置所有弹性盒子
*/
```

<img src="./images/16.png" />

```css
.item_1 {
  /* 
    通过 flex-shrink 属性可以改变盒子缩减行为, 它的默认值为1, 即每个盒子的缩减比例一致
    可以将 flex-shrink 属性的值设置为 0, 表示不缩减当前盒子, 增加其他盒子的缩减比例
  */
  flex-shrink: 0;
}
```

<img src="./images/17.png" />

```css
.item {
  width: 500px;
  flex-shrink: 0;
}
```

<img src="./images/18.png" />

```css
.container {
  /* 当弹性容器的宽度不够时, 可以通过设置 flex-wrap: wrap 让弹性盒子换行显示 */
  flex-wrap: wrap;
}
```

<img src="./images/19.png" />

```css
.container {
  /* 通过 align-content 属性可以设置弹性盒子的行对齐方式 */
  align-content: flex-start;
}
```

<img src="./images/20.png" />

```css
/* 通过 order 属性可以调整元素的显示顺序 */
.item_1 {
  order: 3;
}
.item_2 {
  order: 1;
}
.item_3 {
  order: 2;
}
```

<img src="./images/21.png" />

```css
.container {
  /*
    通过设置 flex-direction 属性可以调整主轴方向, 默认值为 row, 即主轴方向为水平, 盒子从左到右排列
    flex-direction: column 设置主轴方向为垂直, 盒子从上到下排列
    flex-direction: column-reverse 设置主轴方向为垂直, 盒子从下到上排列
    flex-direction: row-reverse 设置主轴方向为水平, 盒子从右到左排列
  */
  flex-direction: column;
  /* 设置盒子在主轴方向居中对齐 (垂直) */
  justify-content: center;
  /* 设置盒子在侧轴方向居中对齐 (水平) */
  align-items: center;
}
```

<img src="./images/22.png" />

## 10. 定位

```html
<style>
  /*
    定位是指确定元素的位置
    元素位置的调整可以有不同的参数对象
    可以参数自身, 可以参考有定位的父级, 可以参数浏览器窗口
    css 中可以通过 position 属性来确认元素位置调整时的参数对象
  */

  /*
    静态定位 position: static; 
    元素在不设置定位时, 元素的 position 属性值就是 static
    一般在使用它时都是在取消该元素的其他定位特性.
  */

  /* 
    相对定位 position: relative
    相对定位的元素是相对于元素自身在文档流中原本的位置进行定位
    采用相对定位的元素没有脱离文档流, 所以当相对定位的元素被调整位置时, 原有位置会被保留, 不会影响文档流中的其他元素的位置
    一般在进行网页布局时, 极少改变相对定位元素的位置
    极大多数情况下, 相对定位的元素都是为绝对定位的元素提供位置参考
  */

  /* 
    绝对定位 position: absolute
    绝对定位的元素参考它最近的有定位的父级元素进行定位, 该定位可以是 sticky relative absolute fixed
    若没有定位父级, 则参考窗口元素的位置进行定位
    设置了绝对定位的元素会脱离正常的文档流, 元素原有位置将会被其他元素占据，可以使用绝对定位实现盒子堆叠效果
  */

  /*
    固定定位 position: fixed;
    设置了固定定位的元素, 它的位置参考浏览器窗口, 在页面内容滚动时, 它的位置不会改变。
    设置了固定定位的元素会脱离文档流, 原有位置将会被其他元素占据。
  */

  /*
    黏性定位 position: sticky
    相对于父级进行固定定位, 当父级元素出现在窗口中并进行滚动时, 粘性定位的元素出现 fixed 定位效果
    当父级元素离开窗口后, 粘性定位随父级元素离开窗口
    注意父级元素不需要设置任何定位
  */

  .outer {
    width: 800px;
    height: 2000px;
    background-color: #ccc;
  }
  .inner {
    width: 200px;
    height: 100px;
    background-color: skyblue;
    position: sticky;
    top: 100px;
    left: 0;
  }
  .box {
    width: 800px;
    height: 2000px;
    background-color: purple;
  }
</style>

<div class="outer">
  <div class="inner"></div>
</div>
<div class="box"></div>
<!-- 问题：两个元素, 一个固定定位, 一个绝对定位, 它们两个都相对于窗口定位, 它们有什么区别。 -->
```

## 11. 盒子水平垂直居中

```html
<div class="box"></div>
```

```css
/* 知道元素宽高的情况下 */
.box {
  width: 100px;
  height: 100px;
  background: skyblue;
  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -50px;
  margin-top: -50px;
}
```

```css
/* 不知道元素宽高的情况下 */
.box {
  width: 100px;
  height: 100px;
  background: skyblue;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
```

```css
/* 知道元素宽高的情况下 */
.box {
  width: 100px;
  height: 100px;
  background: skyblue;
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: auto;
}
```

```html
<style>
  .wrap {
    width: 100%;
    height: 500px;
    border: 2px solid skyblue;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .box {
    width: 100px;
    height: 100px;
    background: skyblue;
  }
</style>
<div class="wrap">
  <div class="box"></div>
</div>
```

```html
<style>
  .wrap {
    width: 800px;
    height: 500px;
    border: 2px solid skyblue;
    display: table-cell;
    text-align: center;
    vertical-align: middle;
  }
  .box {
    width: 100px;
    height: 100px;
    display: inline-block;
    background: skyblue;
  }
</style>
<div class="wrap">
  <div class="box"></div>
</div>
```

## 12. 多行文本垂直居中

```html
<style>
  .wrap {
    width: 100px;
    height: 500px;
    border: 2px solid skyblue;
    display: table;
  }
  .box {
    width: 100%;
    height: 100%;
    display: table-cell;
    vertical-align: middle;
  }
</style>
<div class="wrap">
  <div class="box">
    多行文本多行文本多行文本多行文本多行文本多行文本多行文本多行文本多行文本多行文本多行文本多行文本多行文本多行文本
  </div>
</div>
```

## 13. 行高如何继承

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      body {
        font-size: 20px;
        /* 直接被子元素继承, 即子元素的行高就是 20px */
        /* line-height: 20px; */

        /* 子元素继承该行高后, 会使用子元素字体大小乘以行高比例, 16 * 1.4 = 24 */
        /* line-height: 1.5; */

        /*  当行高值写成百分比以后, 会先使用当前字体大小乘以该百分比, 得到的值会被子元素继承, 20 * 2 = 40px */
        line-height: 200%;
      }
      .wrap {
        font-size: 16px;
      }
    </style>
  </head>
  <body>
    <div class="wrap">这是一段文字</div>
  </body>
</html>
```

## 14. rem 单位

rem 是 css 中的长度单位用于实现移动端适配。

移动端适配是指页面元素的宽、高都要随着设备的宽度进行等比缩放，即移动设备的宽度大，页面元素大，移动设备的宽度小，页面元素小。

<div style="overflow:hidden">
	<img src="./images/23.png" align="left" width="30%" style="float:left; margin-right: 10px;"/>
	<img src="./images/24.png" align="left" width="30%" style="float: left"/>
</div>

rem 是一个相对单位，相对于根元素的字体大小进行计算。

比如根元素的字体大小是 20px，那么 1rem 就等于 20px，那么宽度 5rem 和高度 3rem 的盒子最终的宽高就为 100px，高度就为 60px。

```css
html {
  font-size: 20px;
}
.box {
  /* 100px */
  width: 5rem;
  /* 60px */
  height: 3rem;
}
```

要实现移动端适配效果，rem 单位需要和 css 中的媒体查询进行配合使用。

通过媒体查询检测设备的视口宽度，针对不同的设备视口宽度为根元素设置不同的字号大小。

如果设备视口宽度较大就为其设置较大的字号大小，如果设备的视口宽度较小就为其设置较小的字号大小。

```css
@media (width: 375px) {
  html {
    font-size: 20px;
  }
}

@media (width: 414px) {
  html {
    font-size: 30px;
  }
}
```

在真实的项目开发中针对不同的设备的视口宽度，根元素的字号大小要如何进行设置呢？

在 rem 移动端适配方案中，我们通常将网页宽度等分为十份，然后将根元素的字号大小设置为视口宽度的十分之一，这样就将视口宽度和字号进行了关联。

```css
@media (width: 320px) {
  html {
    font-size: 32px;
  }
}

@media (width: 375px) {
  html {
    font-size: 37.5px;
  }
}

@media (width: 414px) {
  html {
    font-size: 41.4px;
  }
}
```

设计稿的宽度是 750px，设计稿中有一个盒子的宽高分别为 100px 和 50px，如何将它们转换为 rem 单位？

我们可以将设计稿的宽度看成是某一个移动设备的视口宽度，那么在该视口宽度下根元素的字号大小为 75px。

我们只需要使用 100px 除以 75px ，50px 除以 75px，就可以得到盒子的对应的宽高 rem 值。

```css
@media (width: 750px) {
  html {
    font-size: 75px;
  }
}
.box {
  /* 盒子原本的像素值除以根元素字号大小: 100 / 75 = 1.333   50 / 75 = 0.666 */
  width: 1.333rem;
  height: 0.666rem;
  background: skyblue;
}
```

通过 VSCode 编辑器中的 [px to rem & rpx & vw (cssrem)](https://marketplace.visualstudio.com/items?itemName=cipchk.cssrem) 插件自动计算 rem 值。

先在 VSCode 编辑器中安装该插件，然后在 VSCode 编辑器的配置文件中设置参考的根元素字号大小

触发转换的快捷键默认为 Alt+Z

```json
{
  "cssrem.rootFontSize": 75
}
```

改进媒体查询以适配所有移动端设备

```css
@media (min-width: 320px) {
}
@media (min-width: 481px) {
}
@media (min-width: 641px) {
}
@media (min-width: 961px) {
}
@media (min-width: 1025px) {
}
@media (min-width: 1281px) {
}
```

也可以通过 flexible.js 替换媒体查询，该 js 可以动态获取设备的视口宽度，为根元素动态设置十分之一的字号大小。

```javascript
(function flexible(window, document) {
  var docEl = document.documentElement;
  var dpr = window.devicePixelRatio || 1;

  // adjust body font size
  function setBodyFontSize() {
    if (document.body) {
      document.body.style.fontSize = 12 * dpr + "px";
    } else {
      document.addEventListener("DOMContentLoaded", setBodyFontSize);
    }
  }
  setBodyFontSize();

  // set 1rem = viewWidth / 10
  function setRemUnit() {
    var rem = docEl.clientWidth / 10;
    docEl.style.fontSize = rem + "px";
  }

  setRemUnit();

  // reset rem unit on page resize
  window.addEventListener("resize", setRemUnit);
  window.addEventListener("pageshow", function (e) {
    if (e.persisted) {
      setRemUnit();
    }
  });

  // detect 0.5px supports
  if (dpr >= 2) {
    var fakeBody = document.createElement("body");
    var testElement = document.createElement("div");
    testElement.style.border = ".5px solid transparent";
    fakeBody.appendChild(testElement);
    docEl.appendChild(fakeBody);
    if (testElement.offsetHeight === 1) {
      docEl.classList.add("hairlines");
    }
    docEl.removeChild(fakeBody);
  }
})(window, document);
```

## 15. 视口单位

vw 和 vh 是 css 中的一个相对的长度单位，被称之为视口单位。

vw 就是 viewport width，表示它相对于视口的宽度进行计算。

vh 就是 viewport height，表示它相对于视口的高度进行计算。

1vw = 1/100 视口宽度，如果视口的宽度是 375px，那么 1vw = 3.75px。

1vh = 1/100 视口高度，如果视口的高度是 667px，那么 1vh = 6.76px。

```css
.box {
  /* 如果视口宽度是 375, 50 * 3.75 = 187.5  30 * 3.75 = 112.5 */
  width: 50vw;
  height: 30vw;
  background: skyblue;
}
```

```css
.box {
  /* 如果视口高度是 667, 50 * 6.67 = 333.5  30 * 6.67 = 200.1 */
  width: 50vh;
  height: 30vh;
  background: skyblue;
}
```

设计稿的宽度是 750px，设计稿中有一个盒子的宽高分别为 100px 和 50px，如何将它们转换为 vw 单位？

我们可以将设计稿的宽度看成是某一个移动设备的视口宽度，那么在该视口宽度下 1vw = 7.5px。

我们只需要使用 100px 除以 7.5px ，50px 除以 7.5px，就可以得到盒子的对应的宽高 vw 值。

```css
.box {
  /* 设计稿宽度 750, 100 / 7.5 = 13.333vw  50 / 7.5 = 6.666vw */
  width: 13.333vw;
  height: 6.666vw;
  background: skyblue;
}
```

注意一般在使用视口单位布局时，一个盒子的宽高一般不会混用 vw 和 vh，一般会基于 vw。

通过配置 cssrem 插件启用对 vw 的支持，使插件辅助我们计算最终的 vw 值。

```json
{
  "cssrem.vw": true,
  "cssrem.vwHover": true
}
```
