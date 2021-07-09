---
title: 17、前端：CSS3
urlname: usglwy
date: '2021-07-09 20:39:35 +0800'
tags: []
categories: []
---

# 1、初识 CSS3

### 本章目标：

会使用行内样式、内部样式表和外部样式表三种方式为 HTML5 文档添加 CSS 样式 会使用 CSS3 的基本选择器设置字体大小和颜色
会使用复合选择器为特定的网页元素添加 CSS 样式会使用 CSS3 高级选择器为网页元素添加 CSS 样式

1.  **、什么是 CSS**

Cascading Style Sheet 级联样式表。
**表现**HTML 或 XHTML 文件样式的计算机**语言。**
包括对字体、颜色、边距、高度、宽度、背景图片、网页定位等设定。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834378672-e0d1b597-289a-445d-a6f2-f9f4bfc56004.png#)

### 说明：

首先介绍什么是 CSS
然后对比讲解使用 CSS 和没有使用 CSS 的两个相同的 HTML 代码页面显示效果，说明 CSS 的重要性 最后根据图说明 CSS 在网页中的应用

## 、CSS 的发展史

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834379234-052f3f1e-92da-4895-a20d-6aa838da07db.jpeg#)

CSS1.0 读者可以从其他地方去使用自己喜欢的设计样式去继承性地使用样式；
CSS2.0 融入了 DIV+CSS 的概念，提出了 HTML 结构与 CSS 样式表的分离
CSS2.1 融入了更多高级的用法，如浮动，定位等。
CSS3.0 它包括了 CSS2.1 下的所有功能，是目前最新的版本，它向着模块化的趋势发展，又加了很多使用的新技术，如字体、多背景、圆角、阴影、动画等高级属性，但是它需要高级浏览器的支持。
由于现在 IE 6、IE 7 使用比例已经很少，对市场企业进行调研发现使用 CSS3 的频率大幅增加，学习 CSS3
已经成为一种趋势，因此本书会讲解最新的 CSS3 版本
本课程中主要讲解 css2.1 和 css3

### CSS 的优势

内容与表现分离
网页的表现统一，容易修改
丰富的样式，使得页面布局更加灵活
减少网页的代码量，增加网页的浏览速度，节省网络带宽运用独立于页面的 CSS，有利于网页被搜索引擎收录

## 、CSS 的基本语法

首先讲解 CSS 的基本语法结构，由选择器和声明构成
然后对照具体的样式详细讲解语法，强调声明必须在{ }中最后说明基本 W3C 的规范，每条声明后的;都要写上

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834379730-209248aa-b260-4c98-b0fb-f0bcbb69a57c.jpeg#)

### Style 标签

讲解 CSS 样式如何在 HTML 中应用，引入 style 标签的应用讲解 style 标签，说明 type=“text/css 的用法
说明 style 标签在 HTML 文档中的位置，在与之间
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834380180-d5a873bc-dbab-4d3d-ac13-e700b72a646e.png#)

## 、引入 CSS 方式

行内样式
使用 style 属性引入 CSS 样式

1. <h1 style="color:red;">style属性的应用</h1>
1. <p style="font-size:14px; color:green;">直接在HTML标签中设置的样式</p>

使用 style 属性设置 CSS 样式仅对当前的 HTML 标签起作为，并且是写在 HTML 标签中的
这种方式不能起到内容与表现相分离，本质上没有体现出 CSS 的优势，因此不推荐使用。

内部样式表
CSS 代码写在 的

<head>
<style>

标签中

1. <style>
1. h1{color: green; }
1. </style>

优点：方便在同页面中修改样式
缺点：不利于在多页面间共享复用代码及维护，对内容与样式的分离也不够彻底 引出外部样式表

外部样式表
CSS 代码保存在扩展名为.css 的样式表中
HTML 文件引用扩展名为.css 的样式表，有两种方式
**链接式**（使用的最多）
签链接外部样式表，并讲解各参数的含义，
使 用 <link> 标

<head> 标签中
<link>

标签必须放在

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834380463-ce262573-7f71-4611-a5ae-b0e3d196a80c.jpeg#)

### 导入式

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834380755-fc9f6cd9-94ca-4577-9913-08151b853dce.png#)使用@import 导入外部样式表

链接式与导入式的区别

      1. <link/> 标签是属于XHTML范畴的，@import是属于CSS2.1中特有的。
      1. 使用 <link/> 链接的CSS是客户端浏览网页时先将外部CSS文件加载到网页当中，然后再进行编译显示，所以这种情况下显示出来的网页与用户预期的效果一样，即使网速再慢也一样的效果。
      1. 使用@import导入的CSS文件，客户端在浏览网页时是先将HTML结构呈现出来，再把外部CSS文件

<link/>
加载到网页当中，当然最终的效果也与使用	链接文件效果一样，只是当网速较慢时会
先显示没有CSS统一布局的HTML网页，这样就会给用户很不好的感觉。这个也是现在目前大多少网站采用链接外部样式表的主要原因。

      1. [由于@import是属于CSS2.1中特有的，因此对于不兼容](mailto:由于@import是属于CSS2.1中特有的)CSS2.1的浏览器来说就是无效的。

### CSS 样式优先级

1 行内样式>内部样式表>外部样式表
2
3 就近原则：越接近标签的样式优先级越高

【学员练习】
使用标题标签和段落标签制作李白的诗《望庐山瀑布》，诗正文字体颜色为绿色，字体大小为 14px
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834381240-635ce5e3-3717-4d8d-9827-523342fd0a3d.jpeg#)

## 、CSS 基本选择器

标签选择器
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834381775-80d32d07-7959-4387-bbb5-c9b62616fc74.png#)
1 HTML 标签作为标签选择器的名称 2 <h1>…<h6>、<p>、<img/>

类选择器
一些特殊的实现效果，单纯使用标签选择器不能实现，从而引出类选择器

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834382329-6f351616-1a88-4f97-b149-7a60c83c40a8.jpeg#)
ID 选择器
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834382824-c26c0f6f-1024-4626-a943-a848f06b3e8c.png#)ID 选择器的名称就是 HTML 中标签的 ID 名称，ID 全局唯一

### 小结

标签选择器直接应用于 HTML 标签类选择器可在页面中多次使用
ID 选择器在同一个页面中只能使用一次

### 基本选择器的优先级

1 ID 选择器>类选择器>标签选择器

标签选择器是否也遵循“就近原则”？

### 不遵循，无论是哪种方式引入 CSS 样式，一般都遵循 ID 选择器 > class 类选择器 > 标签选择器的优先级

1.  **、CSS 高级选择器**

**1、层次选择器**
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834383346-bf352c55-8b71-43bb-ac45-a3bdb65681f3.jpeg#)

**后代选择器**
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834383856-50165848-2a10-4e34-8dd9-6f9c97a49603.png#)

1. body p{
1. background: red; 3 }

**后代选择器两个选择符之间必须要以空格隔开，中间不能有任何其他的符号插入 \*\***子选择器\*\*

1. body>p{
1. background: pink; 3 }

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834384403-78af92a0-3c11-43d5-bf98-b86823827e7a.png#)

**相邻兄弟选择器**
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834384866-e1161642-a6c9-4734-a30c-64b676a79b6b.png#)

1. .active+p {
1. background: green; 3 }

**通用兄弟选择器**
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834385162-b6ccabf8-41f7-4350-be5f-14ee8796a265.png#)

1. .active~p{
1. background: yellow; 3 }

**2、结构伪类选择器**

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834385490-3989e2a5-1d0c-4f00-8fb4-9a83182b683e.png#)

1. <html>
1. <head lang="en">
1. <meta charset="UTF-8">
1. <title>使用CSS3结构伪类选择器</title>
1. </head>

6 <body>
7 <p>p1</p>
8 <p>p2</p>
9 <p>p3</p>

1. <ul>
1. <li>li1</li>
1. <li>li2</li>
1. <li>li3</li> 14	</ul>
1. </body>
1. </html>

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834386142-b5d29c00-772a-473f-956d-a1749feaabf3.jpeg#)标红的重点强调下，其他的可以略讲

| 1   | ul li:first-child{ background: red;}  |
| --- | ------------------------------------- |
| 2   | ul li:last-child{ background: green;} |
| 3   | p:nth-child(1){ background: yellow;}  |
| 4   | p:nth-of-type(2){ background: blue;}  |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834386655-b4df2ea4-c852-463a-8813-0b06e10c7a88.jpeg#)

### 小结

使用 E F:nth-child(n)和 E F:nth-of-type(n)的 关键点
E F:nth-child(n)在父级里从一个元素开始查找，不分类型 E F:nth-of-type(n)在父级里先看类型，再看位置

**3、属性选择器**
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834387222-e1028003-de66-40fc-a63c-0cd54015480b.jpeg#)

### E[attr]属性选择器

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834387641-a524e04d-e5af-4774-a099-a296d42030fe.jpeg#)

1. a[id] {
1. background: yellow; 3 }

**E[attr=val]属性选择器**

1. a[id=first] {
1. background: red; 3 }

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834388111-b3b4b177-e035-4db9-90d5-22556d49092d.jpeg#)

**E[attr*=val]属性选择器**

1. a[class*=links] {
1. background: red; 3 }

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834388634-2be0d710-b02a-433e-9d7a-e3ec25738210.jpeg#)

**E[attr^=val]属性选择器**
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834389163-6a408e0f-8291-402f-8f13-fd7723b3d10e.jpeg#)

1. a[href^=http] {
1. background: red; 3 }

**E[attr$=val]属性选择器**
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834389729-2fa28be7-4bf0-4dca-bc64-29b1f6a31af8.jpeg#)

1. a[href$=png] {
1. background: red; 3 }

   1. **、小结**

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834390260-79a07d55-43c3-4c7c-87f0-121a8d35f095.jpeg#)

**2、美化网页元素**

**本章目标：**
会使用 CSS 设置字体样式和文本样式会使用 CSS 设置超链接样式
会使用 CSS 设置列表样式会使用 CSS 设置背景样式会使用 CSS 设置渐变效果

## 、为什么使用 CSS

【查看淘宝页面，让学员观察，重点记住了什么东西】因此使用 CSS 样式美化网页文本具有如下意义。

1. 有效的传递页面信息
1. 使用 CSS 美化过的页面文本，使页面漂亮、美观，吸引用户
1. 可以很好的突出页面的主题内容，使用户第一眼可以看到页面主要内容
1. 具有良好的用户体验

< span>标签
< span>标签 的作用：能让某几个文字或者某个词语凸显出来，从而添加对应的样式！

1 <p>好好学习，<span>天天向上</span></p>

## ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834390781-ac334b8e-87ef-4b17-b876-094525ef1b90.jpeg#)、字体样式

**字体类型 **font-family

1. p{font-family:Verdana,"楷体";}
1. body{font-family: Times,"Times New Roman", "楷体";}

同时设置中文和英文时，计算机如何识别中英文不同类型
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834391243-3aeb8620-cecc-4913-aebf-d8e62fbc0d10.jpeg#)

**字体大小 **font-size
单位
px（像素）
em、rem、cm、mm、pt、pc

| 1   | h1{font-size:24px;}     |
| --- | ----------------------- |
| 2   | h2{font-size:16px;}     |
| 3   | h3{font-size:2em;}      |
| 4   | span{font-size:12pt;}   |
| 5   | strong{font-size:13pc;} |

字体风格 font-style
normal、italic 和 oblique

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834392680-1902ed92-ecfd-48ae-80ed-2520ee24af4d.jpeg#)

字体的粗细 font-weight
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834392979-ad06e0a1-0ff3-4a25-b370-4c6a4579abf1.png#)

字体属性 font
字体属性的顺序：字体风格 → 字体粗细 → 字体大小 → 字体类型

1. p span{
1. font:oblique bold 12px "楷体"; 3 }

## 、文本样式

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834393393-ffd2514e-a2d2-4199-85cd-9df54a292524.jpeg#)

### 文本颜色 color

RGB
十六进制方法表示颜色：前两位表示红色分量，中间两位表示绿色分量，最后两位表示蓝色分 量
rgb(r,g,b) : 正整数的取值为 0 ～ 255
RGBA
在 RGB 基础上增加了控制 alpha 透明度的参数，其中这个透明通道值为 0 ～ 1

| 1   | color:#A983D8;           |
| --- | ------------------------ |
| 2   | color:#EEFF66;           |
| 3   | color:rgb(0,255,255);    |
| 4   | color:rgba(0,0,255,0.5); |

排版文本段落
水平对齐方式：text-align 属性
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834393745-30539e16-6b02-4b33-90a3-920d62424407.png#)
首行缩进：text-indent：em 或 px 行高：line-height：px

文本修饰和垂直对齐
文本装饰：text-decoration 属性（后面的讲解中会大量用到）

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834394088-c9346c3f-6ca7-4a6e-86c7-a5363d7c0dfe.jpeg#)

垂直对齐方式：vertical-align 属性：middle、top、bottom
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834394478-a9f0c79f-d142-496c-8126-7a38a68c9af5.jpeg#)

## ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834394844-dbe213ee-44d2-4628-ba27-223576cb6cac.jpeg#)、文本阴影

text-shadow 属性在 CSS2.0 中出现，但迟迟未被各大浏览器所支持，因此在 CSS2.1 中被废弃，如今在
CSS3 中得到了各大浏览器的支持！

【学员练习】

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834395368-cf58af85-760d-4262-a87e-e6ad7c0aa015.jpeg#)

## 、超链接伪类

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834395892-0b768c46-b58a-4e5e-8d56-a6782a6bdcaf.jpeg#)

### 使用 CSS 设置超链接

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834396424-07e26687-6788-41d3-98cf-488d2fb54bd1.jpeg#)
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834396797-03399347-0395-4249-8c96-693197ab76ec.png#)
实际网页开发中通常只设置两种状态，一是 a{color:#333;}，一是 a:hover { color:#B46210;}

## 、列表样式

list-style-type list-style-image
list-style-position list-style
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834397098-213325d1-e467-4333-bcc8-66d1f9b6fcf8.png#)

上网时大家都会看到在浏览的网页中用到列表时很少使用 CSS 自带的列表标记，而是使用设计的图标， 那么大家会想使用 list-style-image 就可以了。可是 list-style-position 不能准确地定位图像标记的位置， 通常，网页中图标的位置都是非常精确的。在实际的网页制作中，通常使用 list-style 或 list-style-type 设 置项目无标记符号，然后通过背景图像的方式把设计的图标设置成列表项标记。在网页制作中，list-
style 和 list-style-type 两个属性是大家经常用到的，而另两个属性则不太常用，因此在这里大家牢记 list-
style 和 list-style-type 的用法即可！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834397546-92a6cf52-273e-4242-ba13-f970b113175f.jpeg#)

## 、背景样式

### 常见的背景样式

背景图像
background-image
背景颜色
background-color

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834397979-cd145c69-a220-42c8-ae38-e7bbe28e7ebb.jpeg#)设置背景图像 background-image 属性 background-repeat 属性

### 背景定位：background-position 属性

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834398536-58a83066-ae29-4b00-b973-3978bb35a1e8.jpeg#)

这些小三角，实现定位
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834398952-a7b73821-b0ce-45af-a367-5fd1c48e10f6.png#)

### 设置背景

背景属性：background 属性（背景样式简写）

1. .title {
1. font-size:18px;
1. font-weight:bold;
1. color:#FFF;
1. text-indent:1em;
1. line-height:35px;
1. background:#C00 url(../image/arrow-down.gif) 205px 10px no-repeat; 8 }

9 background: 背景颜色 背景图像 背景定位 背景不重复显示

背景尺寸 background-size
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834399318-65c0ff4c-f747-4187-b53c-a5dd500b2951.jpeg#)

## 、CSS 渐变样式

网站推荐：[http://color.oulu.me/](http://color.oulu.me/)

### 线性渐变

颜色沿着一条直线过渡：从左到右、从右到左、从上到下等

### 径向渐变

圆形或椭圆形渐变，颜色不再沿着一条直线变化，而是从一个起点朝所有方向混合

CSS3 渐变兼容
IE 浏览器是 Trident 内核，加前缀：-ms-
Chrome 浏览器是 Webkit 内核，加前缀：-webkit-
Safari 浏览器是 Webkit 内核，加前缀：-webkit-
Opera 浏览器是 Blink 内核，加前缀：-o-
Firefox 浏览器是 Mozilla 内核，加前缀：-moz-

线性渐变
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834399655-0768f0bb-d61b-46bf-9a8e-d4b62385e70d.png#)

兼容 Webkit 内核的浏览器

| 1   | -webkit-linear-gradient | (   | position, | color1, | color2,…) |
| --- | ----------------------- | --- | --------- | ------- | --------- |

## 、小结

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834401331-cc16db82-fb34-49e1-b8b1-175d25c1e7fb.jpeg#)

**3、盒子模型**

### 本章目标

理解盒子模型及其构成会计算盒子模型尺寸
会使用盒子模型的两种解析方式来布局网页会使用圆角属性给网页元素添加圆角效果
会使用盒子阴影属性给网页元素添加阴影效果

## 、什么是盒子模型

讲解盒子模型及属性，并说明边框、外边框和内边框都是四个边，最后介绍盒子模型的立体结构

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834401819-fedd3a95-b55f-45e3-a449-4df8a5e780d4.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834402353-8d336a69-f2d6-4786-bce8-f835f441862a.jpeg#)

## 、边框

边框颜色 border-color
边框颜色设置方式与文本颜色对比讲解，都是使用十六进制强调同时设置 4 个边框颜色时，顺序为上右下左
详细讲解分别上、下、左、右各边框颜色的不同设置方式，及属性值的顺序

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834402858-1165e328-d368-429e-a6cc-ad7842747d0e.jpeg#)

边框粗细 border-width
thin medium thick
像素值

| 1   | border-top-width:5px;         |
| --- | ----------------------------- |
| 2   | border-right-width:10px;      |
| 3   | border-bottom-width:8px;      |
| 4   | border-left-width:22px;       |
| 5   | border-width:5px ;            |
| 6   | border-width:20px 2px;        |
| 7   | border-width:5px 1px 6px;     |
| 8   | border-width:1px 3px 5px 2px; |

边框样式 border-style none
hidden dotted dashed solid double

| 1   | border-top-style:solid;                  |
| --- | ---------------------------------------- |
| 2   | border-right-style:solid;                |
| 3   | border-bottom-style:solid;               |
| 4   | border-left-style:solid;                 |
| 5   | border-style:solid ;                     |
| 6   | border-style:solid dotted;               |
| 7   | border-style:solid dotted dashed;        |
| 8   | border-style:solid dotted dashed double; |

border 简写
同时设置边框的颜色、粗细和样式

| 1   | border:1px solid #3a6587; |
| --- | ------------------------- |
| 2   | border: 1px dashed red;   |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834403261-3ce69f6e-7a5a-4fed-b002-f9a8f90e349f.png#)

## 、内外边距

外边距 margin
margin-top margin-right margin-bottom margin-left

| 1   | margin-top: 1 px         |
| --- | ------------------------ |
| 2   | margin-right : 2 px      |
| 3   | margin-bottom : 2 px     |
| 4   | margin-left : 1 px       |
| 5   | margin :3px 5px 7px 4px; |
| 6   | margin :3px 5px;         |
| 7   | margin :3px 5px 7px;     |
| 8   | margin :8px;             |

外边距的妙用：网页居中对齐

1 margin:0px auto;

### 网页居中对齐的必要条件

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834403524-f1554e2c-3899-482a-bf02-68eb6d661671.jpeg#)块元素固定宽度

内边距 padding
padding-left padding-right padding-top padding-bottom

| 1   | padding-left:10px;          |
| --- | --------------------------- |
| 2   | padding-right: 5px;         |
| 3   | padding-top: 20px;          |
| 4   | padding-bottom:8px;         |
| 5   | padding:20px 5px 8px 10px ; |
| 6   | padding:10px 5px;           |
| 7   | padding:30px 8px 10px ;     |
| 8   | padding:10px;               |

## 、盒子型模尺寸

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834403905-1195c422-cc01-479f-9b03-bbdc568f7dc1.jpeg#)

### box-sizing

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834404365-7195463a-0963-4b43-a491-643e5ef5410e.jpeg#)

1. <!DOCTYPE html>
1. <html>
1. <head lang="en">
1. <meta charset="UTF-8">
1. <title>box-sizing</title>
1. <style>
1. div{
1. width: 100px;
1. height: 100px;
1. padding: 5px;
1. margin: 10px;
1. border: 1px solid #000000;
1. box-sizing: border-box;
1. /_box-sizing: content-box; /!_ 默认值*!/*/ 15 }
1. </style>
1. </head>
1. <body>
1. <div></div>
1. </body>
1. </html>

   1. **、圆角边框**

| 1   | border-radius: | 20px | 10px | 50px | 30px; |
| --- | -------------- | ---- | ---- | ---- | ----- |

**四个属性值按顺时针排列**
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834404836-d979917c-1b10-405c-ab8b-bbc8b38888c7.png#)

**border-radius 制作特殊图形：圆形**
利用 border-radius 属性制作圆形的两个要点
元素的宽度和高度必须相同
圆角的半径为元素宽度的一半，或者直接设置圆角半径值为 50%
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834405040-0ae458c5-271f-497b-b4c4-ceda82e085c5.png#)

1. div{
1. width: 100px;
1. height: 100px;
1. border: 4px solid red;
1. border-radius: 50%; 6 }

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17

<!DOCTYPE html>
<html>
<head lang="en">
<meta charset="UTF-8">
<title>border-radius制作圆形</title>
<style>
div{
width: 100px; height: 100px;
border: 4px solid red;
border-radius: 50%;
}
</style>
</head>
<body>
<div></div>

1. </body>
1. </html>

### 使用 border-radius 制作特殊图形：半圆形

利用 border-radius 属性制作半圆形的两个要点
制作上半圆或下半圆时，元素的宽度是高度的 2 倍，而且圆角半径为元素的高度值制作左半圆或右半圆时，元素的高度是宽度的 2 倍，而且圆角半径为元素的宽度值
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834405359-32a4c081-ae4f-459a-86ba-3c0027643d4a.png#)

1. <!DOCTYPE html>
1. <html>
1. <head lang="en">
1. <meta charset="UTF-8">
1. <title>border-radius制作半圆形</title>
1. <style>
1. div{
1. background: red;
1. margin: 30px;

10 }

1. div:nth-of-type(1){
1. width: 100px;
1. height: 50px;
1. border-radius: 50px 50px 0 0; 15 }
1. div:nth-of-type(2){
1. width: 100px;
1. height: 50px;
1. border-radius:0 0 50px 50px; 20 }
1. div:nth-of-type(3){
1. width: 50px;
1. height: 100px;

24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
border-radius:0 50px 50px 0;
}
div:nth-of-type(4){ width: 50px; height: 100px;
border-radius: 50px 0 0 50px;
}
</style>

</head>
<body>
<div></div>
<div></div>
<div></div>
<div></div>
</body>
</html>

### 使用 border-radius 制作特殊图形：扇形

利用 border-radius 属性制作扇形遵循“三同，一不同”原则
“三同”是元素宽度、高度、圆角半径相同
“一不同”是圆角取值位置不同
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834405607-6eae6aef-075f-4fe7-9d6e-825ba066c627.png#)

1. <!DOCTYPE html>
1. <html>
1. <head lang="en">
1. <meta charset="UTF-8">
1. <title>border-radius制作扇形</title>
1. <style>
1. div{
1. background: red;
1. margin: 30px;

10 }

1. div:nth-of-type(1){
1. width: 50px;
1. height: 50px;
1. border-radius: 50px 0 0 0; 15 }

1. div:nth-of-type(2){
1. width: 50px;
1. height: 50px;
1. border-radius:0 50px 0 0; 20 }
1. div:nth-of-type(3){
1. width: 50px;
1. height: 50px;
1. border-radius:0 0 50px 0; 25 }
1. div:nth-of-type(4){
1. width: 50px;
1. height: 50px;
1. border-radius: 0 0 0 50px; 30 }
1. </style>
1. </head>
1. <body>
1. <div></div>
1. <div></div>
1. <div></div>
1. <div></div>
1. </body>
1. </html>

## ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834406501-d91321e3-9cc0-4f2a-a5dc-56f6bbbc0038.jpeg#)、盒子阴影

| 1 | <!DOCTYPE html>

<html>
<head lang="en">
<meta charset="UTF-8">
<title>box-shadow的使用</title>
<style>
div{
width: 100px; height: 100px;
border: 1px solid red; border-radius: 8px; margin: 20px;
/*box-shadow: 20px 10px 10px #06c; |

| /!_内阴影_!/\*/ |
| --------------- | --- | --- |
| 2               |     |     |
| 3               |     |     |
| 4               |     |     |
| 5               |     |     |
| 6               |     |     |
| 7               |     |     |
| 8               |     |     |
| 9               |     |     |
| 10              |     |     |
| 11              |     |     |
| 12              |     |     |
| 13              |     |     |

| 14 |
_!/_/

}
</style>

</head>
<body>
<div></div>
</body>
</html> | /*box-shadow: 0px 0px 20px #06c;

box-shadow: inset 3px 3px 10px #06c; | /!\*只设置模糊半径的阴影

| /_内阴影_/ |
| ---------- |

|
15 | | | |
| 16 | | | |
| 17 | | | |
| 18 | | | |
| 19 | | | |
| 20 | | | |
| 21 | | | |
| 22 | | | |

1.  ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834406986-5ad66758-303a-487f-8907-9d36128e9521.jpeg#)**、小结**

**4、浮动**

### 本章目标：

会使用 display 属性排版网页元素会使用 ﬂoat 属性排版网页元素
会使用 ﬂoat 属性创建横向多列布局
会使用四种防止父级边框塌陷的清除浮动的方法

## 、标准文档流

标准文档流：指元素根据块元素或行内元素的特性按从上到下，从左到右的方式自然排列。这也是元素 默认的排列方式

### 标准文档流组成

块级元素（block）

1 <h1>…<h6>、<p>、<div>、列表

内联元素（inline）

1 <span>、<a>、<img/>、<strong>...

内联标签可以包含于块级标签中，成为它的子元素，而反过来则不成立

## 、display

### ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834407414-84011296-977b-4523-96b5-2fbec79c9508.png#)display 属性

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834407713-28f07c50-8b25-4450-9410-30c9c482e0fb.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834408071-43e7cedc-3d51-476f-ad34-6f7217cd7b85.png#)
1 display:block;
1 display:inline;

image-20191216162826721

1 display:inline-block;

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834408574-7b7910fe-60c6-467f-8f3e-a162e037323f.jpeg#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834409134-ebb24e04-d1fc-4621-9927-92143ec43235.jpeg#)
1 display:none;

display 特性
块级元素与行级元素的转变（**block、inline**） 控制块元素排到一行（**inline-block**）
控制元素的显示和隐藏（**none**）

【学员操作—制作 QQ 会员页面导航】导航背景颜色为黑色半透明效果
鼠标移入“功能特权”等导航信息时文字颜色变为蓝色，无下划线
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834409633-29fb81e1-2dda-4337-9510-f35565345409.jpeg#)“登录”部分信息使用超链接实现，添加圆角边框，鼠标移入字体颜色加深，添加背景颜色为黄色

### 块元素排在一行的方法

可以使用什么属性使块元素排在一行？
inline-block ﬂoat
刚刚学过的 inline-block；然后介绍还有一种方式可以实现让块元素排在一行。引出浮动。

## 、浮动

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834410151-114b6030-6d02-4bae-adb5-b539a3113be1.jpeg#)ﬂoat 属性

1. <body>
1. <div id="father">
1. <div class="layer01"><img src="image/photo-1.jpg" alt="日用品" />

</div>

1. <div class="layer02"><img src="image/photo-2.jpg" alt="图书" /></div>
1. <div class="layer03"><img src="image/photo-3.jpg" alt="鞋子" /></div>
1. <div class="layer04">浮动的盒子……</div> 7	</div>

8 </body>

左浮动

1. .layer01 {
1. border:1px #F00 dashed;
1. float:left; 4 }
1. .layer02 {
1. border:1px #00F dashed;
1. float:left; 8 }

9 .layer03 {

1. border:1px #060 dashed;
1. float:left; 12 }

右浮动

1. .layer01 {
1. border:1px #F00 dashed;
1. float:right; 4 }
1. .layer02 {
1. border:1px #00F dashed;
1. float:right; 8 }

## 、边框塌陷

layer04 设置宽度和右浮动后，为什么边框塌陷了？怎么解决？ 浮动元素脱离标准文档流
清除浮动

### clear 属性

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834410647-196017b2-2994-4f5a-a655-8776048b3877.jpeg#)

1. .layer04 {
1. clear:both; #清除两侧浮动 3 }
1. .layer04 {
1. clear:left; #清除左侧浮动 6 }
1. .layer04 {
1. clear:right; #清除右侧浮动 9 }

**解决父级边框塌陷的方法**
clear 属性可以清除浮动对其他元素造成的影响，可是依然解决不了父级边框塌陷问题，怎么办？ 浮动元素后面加空 div

| 1 | <div id="father">

<div class="layer01"><img src="image/photo-1.jpg"
<div class="layer02"><img src="image/photo-2.jpg"
<div class="layer03"><img src="image/photo-3.jpg"
<div class="layer04">浮动的盒子……</div>
<div class="clear"></div>
</div>
.clear{	clear: both;	margin: 0; padding: 0;} | 
alt="日用品" /></div>
alt="图书" /></div>
alt="鞋子" /></div> |
| --- | --- | --- |
| 2 |  |  |
| 3 |  |  |
| 4 |  |  |
| 5 |  |  |
| 6 |  |  |
| 7 |  |  |
| 8 |  |  |

设置父元素的高度

| 1 | <div id="father">

<div class="layer01"><img src="image/photo-1.jpg"
<div class="layer02"><img src="image/photo-2.jpg"
<div class="layer03"><img src="image/photo-3.jpg"
<div class="layer04">浮动的盒子……</div>
</div>
#father {height: 400px; border:1px #000 solid; } | 
alt="日用品" /></div>
alt="图书" /></div>
alt="鞋子" /></div> |
| --- | --- | --- |
| 2 |  |  |
| 3 |  |  |
| 4 |  |  |
| 5 |  |  |
| 6 |  |  |
| 7 |  |  |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834411185-f7dec0a3-56dd-4872-bab4-3090035b7fbb.jpeg#)父级添加 overﬂow 属性（溢出处理）
hidden 属性值，这个值在网页中经常使用，通常与< div>宽度结合使用设置< div>自动扩展高度， 或者隐藏超出的内容

| 1 | <div id="father">

<div class="layer01"><img src="image/photo-1.jpg"
<div class="layer02"><img src="image/photo-2.jpg"
<div class="layer03"><img src="image/photo-3.jpg"
<div class="layer04">浮动的盒子……</div>
</div>
#father {overflow: hidden;border:1px #000 solid; } | 
alt="日用品" /></div>
alt="图书" /></div>
alt="鞋子" /></div> |
| --- | --- | --- |
| 2 |  |  |
| 3 |  |  |
| 4 |  |  |
| 5 |  |  |
| 6 |  |  |
| 7 |  |  |

父级添加伪类 after

1
2
3
4
5
6
7
8
9
10
11

<div id="father" class="clear">
<div class="layer01"><img src="image/photo-1.jpg" alt="日用品" /></div>
<div class="layer02"><img src="image/photo-2.jpg" alt="图书" /></div>
<div class="layer03"><img src="image/photo-3.jpg" alt="鞋子" /></div>
<div class="layer04">浮动的盒子……</div>
</div>
.clear:after{
content: ''; display: block;
clear: both;
/*在clear类后面添加内容为空*/
/*把添加的内容转化为块元素*/
/*清除这个元素两边的浮动*/
}

小结

### 【清除浮动，防止父级边框塌陷的四种方法】

浮动元素后面加空 div
简单，空 div 会造成 HTML 代码冗余设置父元素的高度
简单，元素固定高会降低扩展性父级添加 overﬂow 属性
简单，下拉列表框的场景不能用父级添加伪类 after
写法比上面稍微复杂一点，但是没有副作用，推荐使用

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834411730-701917cc-946c-4b56-ae9d-3e9189b67633.jpeg#)【学员操作】

## 、inline-block 和 ﬂoat 区别

### display:inline-block

可以让元素排在一行，并且支持宽度和高度，代码实现起来方便位置方向不可控制，会解析空格
IE 6、IE 7 上不支持

### ﬂoat

可以让元素排在一行并且支持宽度和高度，可以决定排列方向
ﬂoat 浮动以后元素脱离文档流，会对周围元素产生影响，必须在它的父级上添加清除浮动的样式

## 、小结

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834412317-6977a21b-4f6a-4af4-a429-db19b64203ce.jpeg#)

**5、定位**

### 本章目标：

会使用 position 定位网页元素
会使用 z-index 属性调整定位元素的堆叠次序

## ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834412815-3150deb3-6a1d-4712-85ff-129aaf51bd9e.jpeg#)、定位在网页中的应用

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834413442-7b813734-79b2-49d4-a3cb-6521b3671f86.jpeg#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834413874-48b0fee5-058e-430c-8faf-bde09b775f36.jpeg#)

1.  **、相对定位**

### position 属性

static：默认值，没有定位

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834414300-8eda25d2-975d-474f-8046-f5a8e45af286.jpeg#)

relative：相对定位
相对自身原来位置进行偏移，偏移设置：top、left、right、bottom
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834414861-8524906f-1534-47d0-a0b5-8afd1eb2ac3f.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834415327-66cb9737-0ab3-4048-8e2b-d1e65f6c8554.png#)

### 相对定位元素的规律

设置相对定位的盒子会相对它原来的位置，通过指定偏移，到达新的位置
设置相对定位的盒子仍在标准文档流中，它对父级盒子和相邻的盒子都没有任何影响 设置相对定位的盒子原来的位置会被保留下来

### 设置第二个盒子右浮动，再设置第一、第二盒子相对定位

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834415589-7fb69955-76a5-4b46-b3f1-045217c5ab98.jpeg#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834415914-dcf3dc56-8dd3-4fea-ac5a-b3d885817169.jpeg#)

1. #first {
1. background-color:#FC9;
1. border:1px #B55A00 dashed;
1. position:relative;
1. right:20px;
1. bottom:20px; 7 }
1. #second {
1. background-color:#CCF;
1. border:1px #0000A8 dashed;
1. float:right;
1. position:relative;
1. left:20px;
1. top:-20px; 15 }

**学员操作：**
使用
和超链接布局页面
每个超链接宽度和高度都是 100px，背景颜色是粉色，鼠标指针移上去时变为蓝色使用相对定位改变每个超链接的位置

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834416362-3c77eef8-60d5-44d1-8bac-1b2b16c7ccf2.png#)

## 、绝对定位

absolute 属性值：偏移设置： left、right、top、bottom
绝对定位：
使用了绝对定位的元素以它最近的一个“已经定位”的“祖先元素” 为基准进行偏移如果没有已经定位的祖先元素，会以浏览器窗口为基准进行定位
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834416633-07e3460b-2887-4710-b815-900374daa759.jpeg#)绝对定位的元素从标准文档流中脱离，这意味着它们对其他元素的定位不会造成影响 元素位置发生偏移后，它原来的位置不会被保留下来

设置了绝对定位但没有设置偏移量的元素将保持在原来的位置。
在网页制作中可以用于需要使某个元素脱离标准流，而仍然希望它保持在原来的位置的情况

## 、固定定位

### ﬁxed 属性值

偏移设置： left、right、top、bottom

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834417005-5e3bd9d5-b3b1-481b-bbab-ca7d42a5e7b1.jpeg#)类似绝对定位，不过区别在于定位的基准不是祖先元素，而是浏览器窗口

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834417511-4ec482f5-1a25-4838-a7db-4bbf0209a330.jpeg#)

## 、定位小结

### 相对定位

相对定位的特性
相对于自己的初始位置来定位
元素位置发生偏移后，它原来的位置会被保留下来
层级提高，可以把标准文档流中的元素及浮动元素盖在下边相对定位的使用场景
相对定位一般情况下很少自己单独使用，都是配合绝对定位使用，为绝对定位创造定位父级而 又不设置偏移量

### 绝对定位

绝对定位的特性
绝对定位是相对于它的定位父级的位置来定位，如果没有设置定位父级，则相对浏览器窗口来 定位
元素位置发生偏移后，原来的位置不会被保留
层级提高，可以把标准文档流中的元素及浮动元素盖在下边设置绝对定位的元素脱离文档流
绝对定位的使用场景
一般情况下，绝对定位用在下拉菜单、焦点图轮播、弹出数字气泡、特别花边等场景

### 固定定位

固定定位的特性
相对浏览器窗口来定位
偏移量不会随滚动条的移动而移动固定定位的使用场景
一般在网页中被用在窗口左右两边的固定广告、返回顶部图标、吸顶导航栏等

## 、z-index 属性

调整元素定位时重叠层的上下位置
z-index 属性值：整数，默认值为 0
设置了 positon 属性时，z-index 属性可以设置各元素之间的重叠高低关系
z-index 值大的层位于其值小的层上方

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834417866-5378b452-63de-4b48-9d6c-7f84fe7d592d.jpeg#)

### ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834418183-72d95b43-170c-4561-ae6b-f0e3428b7789.png#)网页元素透明度

小结
网页中的元素都含有两个堆叠层级
未设置绝对定位时所处的环境，z-index 是 0
设置绝对定位时所处的堆叠环境，此时层的位置由 z-index 的值确定
改变设置绝对定位和没有设置绝对定位的层的上下堆叠顺序，只需调整绝对定位层的 z-index 值即可

## 、小结

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834418544-fe6c0210-f586-47ac-9d13-1956d2b70f5b.jpeg#)

**6、制作网页动画**

### 本章目标

会使用 transform 2D 变形设置网页元素样式会使用 transition 制作过渡动画
会使用 animation 制作网页动画

### 如何在网页中实现动画效果？

1. 动态图片
1. Flash
1. JavaScript

Flash 需要插件支持，文件体积大
从这次课开始学习使用 CSS 代码来完成动画：存在兼容性问题
CSS3 变形
CSS3 过渡
CSS3 动画

## 、CSS 变形

CSS3 变形是一些效果的集合
如平移、旋转、缩放、倾斜效果
简言之，平移就是一个变形，旋转就是一个变形。。。
每个效果都可以称为变形（transform），它们可以分别操控元素发生平移、旋转、缩放、倾斜等变化

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834418861-e5aca168-0faa-48e3-ab94-ba9ef24a01b3.png#)

### 变形函数

translate()：平移函数，基于 X、Y 坐标重新定位元素的位置 scale()：缩放函数，可以使任意元素对象尺寸发生变化 rotate()： 旋 转 函 数 ， 取 值 是 一 个 度 数 值 skew()：倾斜函数，取值是一个度数值

### 2D 位移

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834419178-eb6b7302-cae2-43f0-af8b-f27d3632c0a3.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834419580-8820fd9c-d1e9-41e0-ab0d-ecb1cd726126.png#)

**一个方向上的偏移**
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834420178-81867599-5a14-4d2d-af5f-79f6c7b48259.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834420658-a317ac05-f910-45f5-8d61-10b7fe40353a.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834421233-b261d2c9-eb23-4fc7-958e-811c7b67dd9a.jpeg#)**2D 缩放**

scale()函数能够用来缩放元素大小，该函数包含两个参数值，分别用来定义宽度和高度的缩放比例，默认值为 1，0 ～ 0.99 的任意值都可以使元素缩小，而任何大于 1 的值都能让元素放大。
scale()函数和 translate()函数的语法非常相似，可以只接收一个值，也可以接收两个值，只有一个值
时，第二个值默认和第一个值相等，例如，scale（1，1）元素不会有任何变化，而 scale（2，2）会让 元素放大 2 倍

### 2D 倾斜

**可以仅设置沿着 X 轴或 Y 轴方向倾斜**
skewX（ax）：表示只设置 X 轴的倾斜
skewY（ay）：表示只设置 Y 轴的倾斜

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834421624-b24e5402-de4d-4800-b588-4bdf2a5c7ec1.png#)

### 2D 旋转

1 rotate(a);

参数 a 单位使用 deg 表示
参数 a 取正值时元素相对原来中心顺时针旋转
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834422096-52ece6f9-2d0b-468a-909d-3b8c0ba81df2.png#)

### 小结：

rotate( )函数只是旋转，而不会改变元素的形状
skew( )函数是倾斜，元素不会旋转，会改变元素的形状

## 、CSS 过渡

transition 呈现的是一种过渡，是一种动画转换的过程，如渐现、渐弱、动画快慢等
CSS3 transition 的过渡功能更像是一种“黄油”，通过一些 CSS 的简单动作触发样式平滑过渡

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834422616-3ba2ec81-c261-4696-a1f7-1c8bb4650ca6.jpeg#)

### 过渡属性的使用

过渡属性（ transition-property ） 定义转换动画的 CSS 属性名称
IDENT：指定的 CSS 属性（width、height、background-color 属性等） all：指定所有元素支持 transition-property 属性的样式，一般为了方便都会使用 all

设置过渡中背景颜色，让学员观察到 div 的背景颜色的变化。

过渡所需的时间（ transition-duration ）
定义转换动画的时间长度，即从设置旧属性到换新属性所花费的时间，单位为秒（s）

设置过渡第二个参数，分别设置时间长短，让学员明显观察到这个参数的作用及使用场景

过渡动画函数（ transition-timing-function ）
指定浏览器的过渡速度，以及过渡期间的操作进展情况，通过给过渡添加一个函数来指定动画 的快慢方式
ease：速度由快到慢（默认值） linear：速度恒速（匀速运动）
ease-in：速度越来越快（渐显效果） ease-out：速度越来越慢（渐隐效果）
ease-in-out：速度先加速再减速（渐显渐隐效果）

演示大概 2-3 个动画函数的值，让学员理解第三个参数（动画函数）的作用及使用场景

过渡延迟时间（ transition-delay ）
指定一个动画开始执行的时间，当改变元素属性值后多长时间去执行过渡效果
正值：元素过渡效果不会立即触发，当过了设置的时间值后才会被触发负值：元素过渡效果会从该时间点开始显示，之前的动作被截断 0：默认值，元素过渡效果立即执行

设置第四个参数，过渡延迟时间，让学员能明白过这个参数的作用及使用场景

### 过渡的触发机制

伪类触发
：hover
：active
：focus
：checked
媒体查询：通过@media 属性判断设备的尺寸，方向等
JavaScript 触发：用 JavaScript 脚本触发

**注意：**媒体查询和 JavaScript 的方法在后面课程会详细讲解，现在只需要大家了解即可。重点需要掌握伪 类触发的方法，这种方法也是实际开发中用的比较多的一种

使用 transition 实现过渡动画的使用步骤
在默认样式中声明元素的初始状态样式声明过渡元素最终状态样式，如悬浮状态
在默认样式中通过添加过渡函数，添加一些不同的样式

## 、CSS 动画

animation 动画简介
animation 实现动画主要由两个部分组成
通过类似 Flash 动画的关键帧来声明一个动画
在 animation 属性中调用关键帧声明的动画实现一个更为复杂的动画效果

### CSS3 动画的使用过程

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834423182-b0884117-89e8-432a-8edf-b6f5cd803b60.jpeg#)设置关键帧
@keyframes 的浏览器兼容性

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834423684-76234bbd-f79a-4ca4-8a5e-f52dbb1e8f20.png#)
写兼容的时候浏览器前缀是放在@keyframes 中间例如：@-webkit-keyframes、@-moz- keyframes 调用关键帧
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834424259-dd675e54-e06e-4b18-888e-d7867a1ff531.jpeg#)
动画的播放次数（animation-iteration-count） 值通常为整数，默认值为 1
特殊值 inﬁnite，表示动画无限次播放动画的播放方向（animation-direction）
normal，动画每次都是循环向前播放 alternate，动画播放为偶数次则向前播放
动画的播放状态（animation-play-state）
running 将暂停的动画重新播放
paused 将正在播放的元素动画停下来动画发生的操作（animation-ﬁll-mode）
forwards 表示动画在结束后继续应用最后关键帧的位置
backwards 表示会在向元素应用动画样式时迅速应用动画的初始帧
both 表示元素动画同时具有 forwards 和 backwards 的效果

## 、总结

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834424940-e6ba0ee5-6e7b-4611-a4a2-8cb6f14ab9fb.jpeg#)
