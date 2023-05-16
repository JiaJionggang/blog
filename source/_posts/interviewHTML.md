---
title: HTML面试题
# single_column: true
---

# HTML 高频面试题

## 01. HTML 语义化

① 什么是 HTML 语义化

HTML 是一门标记语言，在这门语言中每一个标记都被赋予了特殊的含义。

开发者在构建页面布局时应使用恰当语义的 HTML 标签进行内容的展示。

```html
<!-- 用于定义标题 -->
<h1></h1>
<!-- 用于定义页面头部区域或 section 区域的页眉 -->
<header></header>
<!-- 用于定义网页导航链接区域 -->
<nav></nav>
<!-- 用于定义页面主体内容, 一个页面只能使用一次 -->
<main></main>
<!-- 用于定义一个页面中的一块自成一体的内容, 可以有自己的 header、footer、section 等 -->
<article></article>
<!-- 在 article 外, 主要用于定义页面侧边栏区域 -->
<!-- 在 article 内, 主要用于定义主要内容的附属内容 -->
<aside></aside>
<!-- 表示有语义化的 div, 用于标记页面中的各个部分 -->
<section></section>
<!-- 用于定义页面底部区域 -->
<footer></footer>
<!-- 用于定义标题组 -->
<hgroup></hgroup>
<!-- 用于标记强调文本 -->
<strong></strong>
<!-- 用于标记一个段落 -->
<p></p>
<!-- 用于标记一个独立的流内容, 比如图像、图标、代码等等 -->
<figure>
  <img src="/media/cc0-images/elephant-660-480.jpg" alt="Elephant at sunset" />
  <figcaption>An elephant at sunset</figcaption>
</figure>
<!-- 用于标记时间 -->
<time>2011-01-28</time>
```

② HTML 语义化有什么好处

(1) 语义化的 HTML 代码使开发者更容易理解，增加了程序的可阅读性便于团队开发和维护

(2) 使搜索引擎能够快速定位网页中的重要内容，爬虫是依赖于标签来确定上下文和各个关键字的权重

(3) 在没有 CSS 样式情况下也能够让页面呈现出清晰的结构 (一些标记自带样式)

(4) 极大程度利用标签的特点优化用户体验，比如 img 标记的 alt 属性和 title 属性、比如 label 标记

## 02. 块级元素与行内元素

① 在 HTML 语言中哪些标记属于块级元素、哪些标记属于行内元素、哪些标记属于行内块元素

display 属性值为 block 或 table 的元素属于块级元素。

```html
<div></div>
<p></p>
<h1></h1>
<ul></ul>
<ol></ol>
<table></table>
<p></p>
```

display 属性值为 inline 的元素属于行内元素。

```html
<span></span>
<a></a>
<b></b>
<strong></strong>
<i></i>
```

display 属性值为 inline-block 的元素属于行内块元素

```html
<img />
<input />
<button></button>
```

② 块级元素有什么特点

(1) 块级元素独占一行

(2) 块级元素的宽度默认为 100%

(3) 块级元素的高度、行高、外边距、内边距可控

(4) 可以包含行内元素可块级元素

③ 行内元素有什么特点

(1) 行内元素可以与其他行内元素同在一行，在一行排不下的情况下才会换行显示

(2) 行内元素可设置水平方向上的内边距和外边距、垂直方向无效

(3) 行内元素不能设置宽度和高度，其宽度和高度由内容自动撑开

(4) 行内元素只能包含其他行内元素或文本

④ 行内块元素有什么特点

(1) 和相邻的行内元素(行内块)在一行上但是中间会有空白的间隙

(2) 可设置宽度、但默认宽度为内容撑开的宽度

(3) 高度、内边距、外边距都可以设置

(4) 行内块元素不能转化为行内元素

⑤ 通过哪些方式可以将行内元素转换为块级元素

```css
display: block;
/* 为行内元素设置 float:left/right 后, 该元素的 display 属性会被设置 block, 且拥有浮动特性。 */
float: left;
float: right;
/* 为行内元素设置决定定位或固定定位时, 会使得行内元素变为块级元素。 */
position: absolute;
position: fixed;
```

## 03. src 属性与 href 属性的区别

src 属性和 href 属性都可以用来引入外部的资源。

---

src 全称 source，通过 src 属性指向的内容会嵌入到文档中标签所在位置比如 js 脚本、img 图片。

当浏览器解析到该元素时，会暂停其它资源下载直到将该资源加载、编译、执行完毕。

正因为该特性所以我们才建议将 js 脚本放置在页面底部加载，防止阻塞页面加载影响用户体验。

---

href 全称 hyper reference 表示超文本引用，用于建立标签与外部资源的关系。

当浏览器解析到该元素时，会和其他资源并行下载，并不会停止对文档的解析，通常用于超链接和样式表的加载。

由于并行加载特性所以我们才建议将样式表文件防止在页面顶部加载，防止出现页面裸奔现象。

```html
<link href="cssfile.css" />
<a href="http://www.webpage.com"></a>
<script src="myscript.js"></script>
<img src="mypic.jpg" />
<video src=""></video>
<audio src=""></audio>
```

## 04. 图像标记的 title 属性与 alt 属性

alt 属性全称 alternate 表示备用，如果图像无法显示浏览器将渲染 alt 指定的内容。

title 属性表示图像的标题，当鼠标移动到图像上时显示 title 属性值中的内容。

```html
<img src="" title="鼠标移入图像时展示的内容" alt="图像无法显示时展示的内容" />
```

## 05. label 标签的作用

label 标签的作用是为使用鼠标的用户改进了可用性，当用户点击 label 标签中的文本时浏览器就会自动将焦点转到和该标签相关联的表单控件上。

```html
<form>
  <label for="male">男</label>
  <input type="radio" name="sex" id="male" />
  <label for="female">女</label>
  <input type="radio" name="sex" id="female" />
</form>
```

## 06. GET 与 POST 的区别

GET 和 POST，两者都是 HTTP 协议中发送请求的方法。

GET 一般用于从服务器端获取数据，POST 一般用于向服务器端传送数据。

GET 和 POST 本质上使用的都是 TCP 链接并无差别，但由于 HTTP 的规定和浏览器/服务器的限制，导致他们在使用过程中会体现出一些区别。

---

① GET 在浏览器回退时是无害的而 POST 会再次提交请求。

② GET 产生的 URL 地址可以被存储为书签而 POST 不可以。

③ GET 请求会被浏览器主动缓存而 POST 不会，除非手动设置。

④ GET 请求只能进行 url 编码而 POST 支持多种编码方式。

⑤ GET 请求参数会被完整保留在浏览器历史记录里而 POST 中的参数不会被保留。

⑥ GET 请求在 URL 中传送的参数是有长度限制的而 POST 没有。

⑦ GET 比 POST 更不安全，因为参数直接暴露在 URL 中，所以不能用来传递敏感信息。

⑧ GET 参数通过 URL 传递，POST 放在请求体中。
