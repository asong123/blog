---
title: CSS3入门学习
urlname: ugov60
date: '2021-07-12 12:53:02 +0800'
tags: []
categories: []
abbrlink: 23613
---

#

# 什么是 CSS

## 第一个 CSS 程序

**style.css**

```css
h1 {
  color: red;
}
```

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>CSS快速入门</title>
    /*外部样式*/
    <link rel="stylesheet" href="css/style.css" />
  </head>
  <body>
    <h1>CSS基础学习</h1>
  </body>
</html>
```

## 导入方式**​**

**style.css**

```html
h1{ /*外部样式*/ color: yellow; }
```

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
    <!-- 内部样式 -->
    <style>
      h1 {
        color: green;
      }
    </style>

    /*外部样式*/
    <link rel="stylesheet" href="css/style.css" />
  </head>
  <body>
    <!-- 行类样式 -->
    <h1 style="color: red">我是标题</h1>
  </body>
</html>
```

**优先级：** **就近原则** ：行内样式离得最近最优先生效，内部和外部样式，看谁离标签越近，谁就生效。
**index2.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
    <style>
      @import url("css/style.css");
    </style>
  </head>
  <body>
    <h1>我在学Java</h1>
  </body>
</html>
```

     首页link和import语法结构不同，前者<link>是html标签，只能放入html源代码中使用，后者可看作为css样式，作用是引入css样式功能。import在html使用时候需要<style type="text/css">标签，同时可以直接“@import url(CSS文件路径地址);”放如css文件或css代码里引入其它css文件。

# 选择器

## 基本选择器

### 1.标签选择器

```html
<style>
  /*标签选择器 会选择到页面上所有的这个标签*/
  h1 {
    color: #fa4ac9;
    background: red;
    border-radius: 10px;
  }
  p {
    font-size: 80px;
  }
</style>
```

### 2. 类选择器

```html
<!--    类选择器的格式 .class的名称 -->
<style>
  .aa {
    color: red;
  }
  .bb {
    color: green;
  }
  .cc {
    color: blue;
  }
</style>
```

### 3. id 选择器

**_  注：** i**d**选择器，**id\_\_必须唯一_**

```html
<style>
  #aaa {
    color: red;
  }
  #bbb {
    color: white;
  }
  #ccc {
    color: yellow;
  }
  #ddd {
    color: blue;
  }
  #eee {
    color: red;
  }
</style>
```

**优先级：**_ id 选择器 > 类选择器 > 标签选择器 ​_

## 层次选择器

### 1. 后代选择器(空格 )

```html
/* 后代选择器：在某个元素的后面 祖爷爷 爷爷 爸爸 你 */ body p{ background: red;
}
```

### 2. 子代选择器(>)

```html
/* 子选择器*/ body>p{ background: aqua; }
```

### 3. 相邻兄弟选择器(+)

```html
<p class="active">p7</p>
<p>p8</p>

/* 兄弟选择器: 只有一个，相邻（向下）*/ .active +p{ background: #fa4ac9; }
```

### 4. 通用选择器（~）

```html
/*通用选择器：当前选择元素的向下的所有兄弟标签*/ .active~p{ background:
blueviolet; }
```

## 结构伪类选择器(:)

```html
/*ul的第一个子元素*/ ul li:first-child{ background: blue; }
/*ul的最后一个子元素*/ ul li:last-child{ background: green; } /*
父元素的第一个*/ p:nth-child(1){ background: blueviolet; }
/*选中父元素下的P元素类型中的第二个*/ p:nth-of-type(2){ background: fuchsia; }
/*移动到该标签，才显示效果*/ a:hover{ background: red; }
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596202240803-2a76f9c0-d57c-47d8-bad3-101d59a65b4a.png#height=127&id=xI48D&margin=%5Bobject%20Object%5D&name=image.png&originHeight=254&originWidth=1400&originalType=binary∶=1&size=11430&status=done&style=none&width=700)

## 属性选择器

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
    <style>
      .demo a {
        float: left;
        display: block;
        height: 50px;
        width: 50px;
        border-radius: 10px;
        background: blue;
        text-align: center;
        color: gray;
        text-decoration: none;
        margin-right: 5px;
        font: bold 20px/50px Arial;
      }

      /*  1. 选择存在id属性的元素    */
      a[id] {
        background: yellow;
      }
      a[id="first"] {
        background: green;
      }
      /* *= 通配*/
      a[class*="links"] {
        background: red;
      }
      /* ^= 以什么开头*/
      a[href^="http"] {
        background: blueviolet;
      }
      /*  $= 以什么结尾 */
      a[href$="pdf"] {
        background: cyan;
      }
    </style>
  </head>
  <body>
    <p class="demo">
      <a href="http:www.baiwu.com" class="links item first" id="first">1</a>
      <a href="" class="links item active" target="_blank" ,title="test">2</a>
      <a href="images/123.html" class="links item active">3</a>
      <a href="images/123.png" class="links item active">4</a>
      <a href="images/123.jpg" class="links item active">5</a>
      <a href="abc">6</a>
      <a href="/a.pdf">7</a>
      <a href="/abc.pdf">8</a>
      <a href="/abc.doc">9</a>
      <a href="/abcd.doc" class="links item last">10</a>
    </p>
  </body>
</html>
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596203418017-c87d2604-43a4-463b-9297-21210592bf39.png#height=60&id=hH94g&margin=%5Bobject%20Object%5D&name=image.png&originHeight=120&originWidth=773&originalType=binary∶=1&size=8860&status=done&style=none&width=386.5)

# 美化网页元素

## 为什么要美化网页

1. 有效的传递页面信息
1. 美化网页、页面漂亮了才能吸引用户
1. 凸显页面的主题
1. 提高用户体验

**span 标签：** 重点要突出的字，使用 span 标签包起来。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
    <style>
      #title1 {
        font-size: 50px;
      }
      #title2 {
        font-size: 50px;
      }
    </style>
  </head>
  <body>
    欢迎学习<span id="title1">Java</span>和<span id="title2">CSS</span>
  </body>
</html>
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596341288444-1bd2d187-5eed-489c-8133-0cecc0e0e212.png#height=48&id=lfnTs&margin=%5Bobject%20Object%5D&name=image.png&originHeight=96&originWidth=406&originalType=binary∶=1&size=9801&status=done&style=none&width=203)

## 字体样式

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
    <style>
      body {
        font-family: Arial，楷体;
        color: red;
      }
      h1 {
        font-size: 50px;
      }
      .p1 {
        font-weight: bold;
      }
    </style>
  </head>
  <body>
    <h1>故事介绍</h1>
    <p class="p1">
      平静安详的元泱境界，每隔333年，总会有一个神秘而恐怖的异常生物重生，它就是魁拔！魁拔的每一次出现，都会给元泱境界带来巨大的灾难！即便是天界的神族，也在劫难逃。在天地两界各种力量的全力打击下，魁拔一次次被消灭，但又总是按333年的周期重新出现。魁拔纪元1664年，天神经过精确测算后，在魁拔苏醒前一刻对其进行毁灭性打击。但谁都没有想到，由于一个差错导致新一代魁拔成功地逃脱了致命一击。很快，天界魁拔司和地界神圣联盟均探测到了魁拔依然生还的迹象。因此，找到魁拔，彻底消灭魁拔，再一次成了各地热血勇士的终极目标。
    </p>
    <p>
      在偏远的兽国窝窝乡，蛮大人和蛮吉每天为取得象征成功和光荣的妖侠纹耀而刻苦修炼，却把他们生活的村庄搅得鸡犬不宁。村民们绞尽脑汁把他们赶走。一天，消灭魁拔的征兵令突然传到窝窝乡，村长趁机怂恿蛮大人和蛮吉从军参战。然而，在这个一切都凭纹耀说话的世界，仅凭蛮大人现有的一块冒牌纹耀，不要说参军，就连住店的资格都没有。受尽歧视的蛮吉和蛮大人决定，混上那艘即将启程去消灭魁拔的巨型战舰，直接挑战魁拔，用热血换取至高的荣誉。
    </p>
    <p>
      When I wake up in the morning，You are all I see;When I think about
      you，And how happy you make me。 You're everything I wanted;You're
      everything I need;I look at you and know;That you are all to me。Barry
      Fitzpatrick
    </p>
  </body>
</html>
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596342063220-f3a65fcf-b6f1-49b5-8fa5-091aca6ea5a9.png#height=186&id=Kdlmn&margin=%5Bobject%20Object%5D&name=image.png&originHeight=372&originWidth=1920&originalType=binary∶=1&size=182319&status=done&style=none&width=960)

## 文本样式

1. 颜色
1. 文本对齐方式
1. 首行缩进
1. 行高
1. 装饰

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
    <!--
    color：
        单词： red,blue
        RGB: #FFFFF
        RGBA: 颜色 + 透明度
         color: rgb(255,0,0);
         color: rgba(255,0,0,0.9);
     text-align: left\center\right; 排版
     text-indent: 2em;      缩进
     height: 300px;
     line-height: 300px;  行高 和块的高度一致，就可以居中了

    -->
    <style>
      h1 {
        color: rgb(255, 0, 0);
        color: rgba(255, 0, 0, 0.9);
        text-align: center;
      }
      .p1 {
        text-indent: 2em;
      }
      .p3 {
        background: blueviolet;
        height: 300px;
        line-height: 300px;
      }
      .L1 {
        text-decoration: underline;
      }
      .L2 {
        text-decoration: line-through;
      }
      .L3 {
        text-decoration: overline;
      }
      img,
      span {
        vertical-align: middle;
      }
      a {
        text-decoration: none;
      }
    </style>
  </head>
  <body>
    <h1>故事介绍</h1>
    <p class="L1">11</p>
    <p class="L2">22</p>
    <p class="L3">22</p>

    <p class="p1">
      平静安详的元泱境界，每隔333年，总会有一个神秘而恐怖的异常生物重生，它就是魁拔！魁拔的每一次出现，都会给元泱境界带来巨大的灾难！即便是天界的神族，也在劫难逃。在天地两界各种力量的全力打击下，魁拔一次次被消灭，但又总是按333年的周期重新出现。魁拔纪元1664年，天神经过精确测算后，在魁拔苏醒前一刻对其进行毁灭性打击。但谁都没有想到，由于一个差错导致新一代魁拔成功地逃脱了致命一击。很快，天界魁拔司和地界神圣联盟均探测到了魁拔依然生还的迹象。因此，找到魁拔，彻底消灭魁拔，再一次成了各地热血勇士的终极目标。
    </p>
    <p>
      在偏远的兽国窝窝乡，蛮大人和蛮吉每天为取得象征成功和光荣的妖侠纹耀而刻苦修炼，却把他们生活的村庄搅得鸡犬不宁。村民们绞尽脑汁把他们赶走。一天，消灭魁拔的征兵令突然传到窝窝乡，村长趁机怂恿蛮大人和蛮吉从军参战。然而，在这个一切都凭纹耀说话的世界，仅凭蛮大人现有的一块冒牌纹耀，不要说参军，就连住店的资格都没有。受尽歧视的蛮吉和蛮大人决定，混上那艘即将启程去消灭魁拔的巨型战舰，直接挑战魁拔，用热血换取至高的荣誉。
    </p>
    <p class="p3">
      When I wake up in the morning，You are all I see;When I think about
      you，And how happy you make me。 You're everything I wanted;You're
      everything I need;I look at you and know;That you are all to me。Barry
      Fitzpatrick
    </p>

    <p class="p4">
      <img src="images/1.png" alt="" />
      <span>adadsadfasdfsadfdasfasd</span>
    </p>
    <a href="dadsada ">asfdsafasdfas</a>
  </body>
</html>
```

## 超链接伪类和阴影

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
    <style>
      a {
        text-decoration: none;
        color: black;
      }
      /*悬浮*/
      a:hover {
        color: orange;
        font-size: 30px;
      }
      /*按住未释放*/
      a:active {
        color: green;
      }
      a:visited {
      }
      /* text-shadow:阴影颜色，水平偏移，垂直偏移，阴影半径*/
      #price {
        text-shadow: #3cc7f5 10px 0px 2px;
      }
    </style>
  </head>
  <body>
    <a href="#">
      <img src="images/2.jpg" alt="" />
    </a>
    <p>
      <a href="">码出高效：Java开发手册</a>
    </p>
    <p>
      <a href="">作者：孤尽老师</a>
    </p>
    <p id="price">￥99</p>
  </body>
</html>
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596376502512-24fc961d-967e-41c5-99df-b64ec1017252.png#height=219&id=wr8fk&margin=%5Bobject%20Object%5D&name=image.png&originHeight=438&originWidth=536&originalType=binary∶=1&size=64710&status=done&style=none&width=268)

## 列表

**style.css**

```css
#nav2 {
  float: left;
  background: white;
  color: white;
  width: 190px;
  background: wheat;
}
.title {
  width: 190px;
  text-align: center;
  font-size: 16px;
  background: #ff5000;
}
ul {
  list-style: none;
  float: left;
  background: wheat;
}
ul li {
  overflow: hidden;
  line-height: 32px;
  height: 32px;
  font-size: 14px;
  font-weight: 400;
  width: 150px;
  color: #666;
}
li:hover {
  background: #ffecc9;
}
a {
  text-decoration: none;
  color: black;
}
a:hover {
  text-decoration: underline;
  color: orange;
}
```

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
    <link rel="stylesheet" href="css/style2.css" />
  </head>
  <body>
    <div id="nav2">
      <h2 class="title">主题市场</h2>
      <ul>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
        <li><a href="">女装</a>/<a href="">内衣</a>/<a href="">家居</a></li>
      </ul>
    </div>
  </body>
</html>
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596417199484-4c07a2e2-4110-4311-b253-b8762c449d0a.png#height=443&id=s8qIg&margin=%5Bobject%20Object%5D&name=image.png&originHeight=886&originWidth=425&originalType=binary∶=1&size=74327&status=done&style=none&width=212.5)

## 背景

```css
background: red url("../images/2.png") 150px 1px no-repeat;

background-image: url("../images/3.png");
background-repeat: no-repeat;
background-position: 120px 2px;
```

## 渐变

**Grabient 官网链接：**[https://www.grabient.com/](https://www.grabient.com/)

# 盒子模型

## 什么是盒子模型

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596423322520-d06cda3f-aac4-472a-8cbf-b2b199469757.png#height=197&id=jYCx9&margin=%5Bobject%20Object%5D&name=image.png&originHeight=197&originWidth=244&originalType=binary∶=1&size=8102&status=done&style=none&width=244)
**margin: 外边距**
**padding:内边距**
**border: 边框**
**Content(内容)： 盒子的内容，显示文本和图像**
**​**

## 边框

### 内外边框

1. 边框的粗细
1. 边框的样式
1. 边框的颜色

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        h1,ul,li,a,body{
            margin: 0px;
            padding: 0px;
            text-decoration: none;
        }
        h2{
            font-size: 16px;
            background-color: #81f5ac;
            color: white;
            line-height: 30px;
            margin: 0 auto;
        }
        #box{
            width: 300px;
            /*粗细 样式 颜色*/
            border: 1px solid #fff4f4;
            border-radius: initial;
            margin: 0 auto;

        }
        form{
            background:#fff;
        }
        input{
            border: 1px solid greenyellow;
        }
        div:nth-of-type(1){
            padding: 5px 3px;
        }
        div:nth-of-type(2){
            padding: 5px 3px;
        }
        div:nth-of-type(3){
            padding: 5px 3px;
        }
    </style>
</head>

<body>
<div id="box">
    <h2>京东会员</h2>
    <form action="#">
        <div>
            <span>账户：</span>
            <input type="text">
        </div>
        <div>
            <span>密码：</span>
            <input type="text">
        </div>
        <div>
            <span>邮箱：</span>
            <input type="text">
        </div>
    </form>
</div>
</body>
</html>
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596809667697-7056780a-66f8-4380-87ec-5f9de1320fbd.png#height=109&id=bh2W3&name=image.png&originHeight=218&originWidth=521&originalType=binary∶=1&size=11152&status=done&style=none&width=260.5)

### 圆角边框

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        div{
            width: 50px;
            height: 50px;
            border: 5px solid yellowgreen;
            border-radius:50px;
        }

        img{
            border-radius:25px;
             box-shadow: 10px 10px 20px yellow;
        }
    </style>
</head>
<body>
    <div>
    </div>
    <img src="2.png" alt="#">
</body>
</html>
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596810163201-45ee9734-fb0f-482c-8b81-f860ff2f1d59.png#height=105&id=WzWIo&name=image.png&originHeight=201&originWidth=155&originalType=binary∶=1&size=11479&status=done&style=none&width=81)![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596810810438-95529454-d433-4141-b507-0b1a9fb6f2a5.png#height=50&id=KAj1h&margin=%5Bobject%20Object%5D&name=image.png&originHeight=99&originWidth=245&originalType=binary∶=1&size=13960&status=done&style=none&width=122.5)

## dispaly 和 浮动

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        div{
            width: 100px;
            height: 30px;
            border: 1px solid #efffd1;
            background: #fafff3;
            display: inline;
        }
        a{
            width: 20px;
            height: 20px;
            border: 1px solid #fafff3;
            color:black;
            display: inline;
            text-decoration: none;
        }
    </style>
</head>
<body>
<div>
    <a href="#">新闻</a>
    <a href="#">hao123</a>
    <a href="#">地图</a>
    <a href="#">视频</a>
    <a href="#">贴吧</a>
    <a href="#">学术</a>
    <a href="#">更多</a>
</div>
</body>
</html>


float: left;
clear: left;
```

### ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596811536460-4e364018-38b8-47f3-803d-99afed53ff39.png#height=54&id=fzCa0&name=image.png&originHeight=54&originWidth=453&originalType=binary∶=1&size=8129&status=done&style=none&width=453)

## 定位

### 相对定位

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body{
            padding: 20px;
        }
        div{
            margin: 10px;
            padding: 5px;
            font-size: 15px;
            line-height: 25px;
        }
        #father{
            border: 1px solid #666;
            padding: 0;
        }
        #first{
            border: 1px dashed #fa4ac9;
            background-color: #fffba3;
            position: relative;
            top:-20px;
        }
        #second{
            border: 1px dashed #deff68;
            background-color: #ff00cd;
            position: relative;
            left:20px;
        }
        #third{
            border: 1px dashed #81f5ac;
            background-color: #ff3068;
            position: relative;
            bottom:-10px;
        }
    </style>
</head>
<body>
<div id="father">
    <div id="first">第一个盒子</div>
    <div id="second">第二个盒子</div>
    <div id="third">第三个盒子</div>
</div>

</body>
</html>
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596812423802-6234236e-e042-411c-85d4-40aab0538227.png#height=130&id=zHB19&margin=%5Bobject%20Object%5D&name=image.png&originHeight=260&originWidth=1874&originalType=binary∶=1&size=32111&status=done&style=none&width=937)

### 绝对定位

### Z-index
