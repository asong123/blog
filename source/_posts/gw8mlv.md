---
title: 18、前端：JavaScript、jQuery
urlname: gw8mlv
date: '2021-07-09 20:40:33 +0800'
tags: []
categories: []
---

**JavaScript**

参考网站：[https://www.liaoxuefeng.com/wiki/1022910821149312](https://www.liaoxuefeng.com/wiki/1022910821149312)

# 1、概述

## 、前言

JavaScript 是世界上最流行的脚本语言，因为你在电脑、手机、平板上浏览的所有的网页，以及无数基于
HTML5 的手机 App，交互逻辑都是由 JavaScript 驱动的。
简单地说，JavaScript 是一种运行在浏览器中的解释型的编程语言。
那么问题来了，为什么我们要学 JavaScript？尤其是当你已经掌握了某些其他编程语言如 Java、C++的情 况下。
简单粗暴的回答就是：因为你没有选择。在 Web 世界里，只有 JavaScript 能跨平台、跨浏览器驱动网页， 与用户交互。
Flash 背后的 ActionScript 曾经流行过一阵子，不过随着移动应用的兴起，没有人用 Flash 开发手机 App， 所以它目前已经边缘化了。相反，随着 HTML5 在 PC 和移动端越来越流行，JavaScript 变得更加重要了。 并且，新兴的 Node.js 把 JavaScript 引入到了服务器端，JavaScript 已经变成了全能型选手。
JavaScript 一度被认为是一种玩具编程语言，它有很多缺陷，所以不被大多数后端开发人员所重视。很多 人认为，写 JavaScript 代码很简单，并且 JavaScript 只是为了在网页上添加一点交互和动画效果。
但这是完全错误的理解。JavaScript 确实很容易上手，但其精髓却不为大多数开发人员所熟知。编写高质 量的 JavaScript 代码更是难上加难。
一个合格的开发人员应该精通 JavaScript 和其他编程语言。如果你已经掌握了其他编程语言，或者你还什 么都不会，请立刻开始学习 JavaScript，不要被 Web 时代所淘汰。

## 、JavaScript 历史

要了解 JavaScript，我们首先要回顾一下 JavaScript 的诞生。
在上个世纪的 1995 年，当时的网景公司正凭借其 Navigator 浏览器成为 Web 时代开启时最著名的第一代 互联网公司。
由于网景公司希望能在静态 HTML 页面上添加一些动态效果，于是叫 Brendan Eich 这哥们在两周之内设计出了 JavaScript 语言。你没看错，这哥们只用了 10 天时间。
为什么起名叫 JavaScript？原因是当时 Java 语言非常红火，所以网景公司希望借 Java 的名气来推广，但事实上 JavaScript 除了语法上有点像 Java，其他部分基本上没啥关系。

## 、ECMAScript

因为网景开发了 JavaScript，一年后微软又模仿 JavaScript 开发了 JScript，为了让 JavaScript 成为全球标 准，几个公司联合 ECMA（European Computer Manufacturers Association）组织定制了 JavaScript 语言的标准，被称为 ECMAScript 标准。

所以简单说来就是，ECMAScript 是一种语言标准，而 JavaScript 是网景公司对 ECMAScript 标准的一种实现。
那为什么不直接把 JavaScript 定为标准呢？因为 JavaScript 是网景的注册商标。
不过大多数时候，我们还是用 JavaScript 这个词。如果你遇到 ECMAScript 这个词，简单把它替换为
JavaScript 就行了。

## 、JavaScript 版本

JavaScript 语言是在 10 天时间内设计出来的，虽然语言的设计者水平非常 NB，但谁也架不住“时间紧，任 务重”，所以，JavaScript 有很多设计缺陷，我们后面会慢慢讲到。
此外，由于 JavaScript 的标准——ECMAScript 在不断发展，最新版 ECMAScript 6 标准（简称 ES6）已经在 2015 年 6 月正式发布了，所以，讲到 JavaScript 的版本，实际上就是说它实现了 ECMAScript 标准的哪个版本。
由于浏览器在发布时就确定了 JavaScript 的版本，加上很多用户还在使用 IE6 这种古老的浏览器，这就导 致你在写 JavaScript 的时候，要照顾一下老用户，不能一上来就用最新的 ES6 标准写，否则，老用户的浏 览器是无法运行新版本的 JavaScript 代码的。
不过，JavaScript 的核心语法并没有多大变化。我们的教程会先讲 JavaScript 最核心的用法，然后，针对
ES6 讲解新增特性。

## 、引入 JavaScript

JavaScript 代码可以直接嵌在网页的任何地方，不过通常我们都把 JavaScript 代码放到 中：
head

1. <html>
1. <head>
1. <script>
1. alert('Hello, world');
1. </script>
1. </head>
1. <body>

8 ...
9 </body>
10 </html> 11
12

由 包含的代码就是 JavaScript 代码，它将直接被浏览器执行。

<script>...</script>

第二种方法是把 JavaScript 代码放到一个单独的.js 文件，然后在 HTML 中通过引入这个文件：

<script src="...">
</script>

1. <html>
1. <head>
1. <script src="/static/js/abc.js"></script>
1. </head>
1. <body>

6 ...

1. </body>
1. </html>

这样， 就会被浏览器执行。
/static/js/abc.js
把 JavaScript 代码放入一个单独的 文件中更利于维护代码，并且多个页面可以各自引用同一
.js
份 文件。
.js

可以在同一个页面中引入多个还可以在页面中多次编写
.js

<script> js代码... </script>

有些时候你会看到

<script>
文件

标签还设置了一个type属性：

，浏览器按照顺序依次执行。



1	<script type="text/javascript"> 2		...
3	</script>

但这是没有必要的，因为默认的
type
JavaScript。
就是 JavaScript，所以不必显式地把
指定为

## 、IDE 推荐

type
可以用任何文本编辑器来编写 JavaScript 代码。这里我们推荐以下几种文本编辑器：
Visual Studio Code

1 微软出的 Visual Studio Code，可以看做迷你版 Visual Studio，免费！跨平台！内置
JavaScript 支持，强烈推荐使用！

Sublime Text

1 Sublime Text 是一个好用的文本编辑器，免费，但不注册会不定时弹出提示框。

Notepad++

1 Notepad++也是免费的文本编辑器，但仅限 Windows 下使用。

WebStorm

1 WebStorm 是 jetbrains 公司旗下一款 JavaScript 开发工具。目前已经被广大中国 JS 开发者誉为“Web 前端开发神器”、“最强大的 HTML5 编辑器”、“最智能的 JavaScript IDE”等。

IDEA

1 Java 开发人员必备，NB！

HBuilder

1 HBuilder 是 DCloud（数字天堂）推出的一款支持 HTML5 的 Web 开发 IDE。HBuilder 的编写用到了
Java、C、Web 和 Ruby。HBuilder 本身主体是由 Java 编写。它基于 Eclipse，所以顺其自然地兼 容了 Eclipse 的插件。

## 、运行 JavaScript

要让浏览器运行 JavaScript，必须先有一个 HTML 页面，在 HTML 页面中引入 JavaScript，然后，让浏览器加载该 HTML 页面，就可以执行 JavaScript 代码。
你也许会想，直接在我的硬盘上创建好 HTML 和 JavaScript 文件，然后用浏览器打开，不就可以看到效果 了吗？
这种方式运行部分 JavaScript 代码没有问题，但由于浏览器的安全限制，以 file:// 开头的地址无法执行如联网等 JavaScript 代码，最终，你还是需要架设一个 Web 服务器，然后以 http:// 开头的地址来正常执行所有 JavaScript 代码。

### 我的第一个 javaScript 程序

1. <html>
1. <head>
1. <script>
1. // 以双斜杠开头直到行末的是注释，注释是给人看的，会被浏览器忽略
1. /_ 在这中间的也是注释，将被浏览器忽略 _/
1. alert('Hello,world');
1. </script>
1. </head>
1. <body>
1. </body>
1. </html>

浏览器将弹出一个对话框，显示“Hello, world”。你也可以修改两个单引号中间的内容，再试着运行。

## 、调试

俗话说得好，“工欲善其事，必先利其器。”，写 JavaScript 的时候，如果期望显示 ABC ，结果却显示
，到底代码哪里出了问题？不要抓狂，也不要泄气，作为小白，要坚信：JavaScript 本身没有问 题，浏览器执行也没有问题，有问题的一定是我的代码。
XYZ
如何找出问题代码？这就需要调试。
怎么在浏览器中调试 JavaScript 代码呢？
首先，你需要安装 Google Chrome 浏览器，Chrome 浏览器对开发者非常友好，可以让你方便地调试
JavaScript 代码。
安装后，随便打开一个网页，然后点击菜单“查看(View)”-“开发者(Developer)”-“开发者工具(Developer Tools)”，浏览器窗口就会一分为二，下方就是开发者工具：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834435947-1ca26a2f-1dbd-4897-b5e1-c3af49508213.jpeg#)

先点击“控制台(Console)“，在这个面板里可以直接输入 JavaScript 代码，按回车后执行。
要查看一个变量的内容，在 Console 中输入 ，回车后显示的值就是变量的内容。
console.log(a);
关闭 Console 请点击右上角的“×”按钮。请熟练掌握 Console 的使用方法，在编写 JavaScript 代码时，经常需要在 Console 运行测试代码。
如果你对自己还有更高的要求，可以研究开发者工具的“源码(Sources)”，掌握断点、单步执行等高级调试技巧。

# 2、快速入门

## 、基本语法

JavaScript 的语法和 Java 语言类似，每个语句以
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834436154-03061f3d-c168-47eb-b150-117806c4c5a0.png#)
;

结束，语句块用 。
{...}

但是，JavaScript 并不强制要求在每个语句的结尾加 ，浏览器中负责执行 JavaScript 代码的引擎会自
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834436354-409028b3-d7f2-44a5-8506-0c40173268da.png#)
;
动在每个语句的结尾补上 。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834436782-51532f2d-4560-4265-aa8a-d61236615a28.png#)
;

让 JavaScript 引擎自动加分号在某些情况下会改变程序的语义，导致运行结果与期望不一致。在本教 程中，我们不会省略;，所有语句都会添加;。
例如，下面的一行代码就是一个完整的赋值语句：

| 1   | var | x   | =   | 1;  |
| --- | --- | --- | --- | --- |

下面的一行代码是一个字符串，但仍然可以视为一个完整的语句：

1 'Hello, world';

下面的一行代码包含两个语句，每个语句用 表示语句结束：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834436949-f4a716f2-1ec2-407a-8c4c-f6b5b4f7e2d3.png#)
;

| 1   | var | x   | =   | 1;  | var | y   | =   | 2;  | //  | 不建议一行写多个语句! |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --------------------- |

语句块是一组语句的集合，例如，下面的代码先做了一个判断，如果判断成立，将执行 有语句：
{...}
中的所

| 1   | if  | (2  | >   | 1)  | {   |
| --- | --- | --- | --- | --- | --- |
| 2   |     | x   | =   | 1;  |     |
| 3   |     | y   | =   | 2;  |     |
| 4   |     | z   | =   | 3;  |     |
| 5   | }   |     |     |     |     |

注意花括号 内的语句具有缩进，通常是 4 个空格。缩进不是 JavaScript 语法要求必须的，但缩进
{...}
有助于我们理解代码的层次，所以编写代码时要遵守缩进规则。很多文本编辑器具有“自动缩进”的功能，可以帮助整理代码。
还可以嵌套，形成层级结构：
{...}

| 1   | if (2 > 1) { |
| --- | ------------ |
| 2   | x = 1;       |
| 3   | y = 2;       |
| 4   | z = 3;       |
| 5   | if (x < y) { |
| 6   | z = 4;       |
| 7   | }            |
| 8   | if (x > y) { |
| 9   | z = 5;       |
| 10  | }            |
| 11  | }            |

JavaScript 本身对嵌套的层级没有限制，但是过多的嵌套无疑会大大增加看懂代码的难度。遇到这种情 况，需要把部分代码抽出来，作为函数来调用，这样可以减少代码的复杂度。
以 开头直到行末的字符被视为行注释，注释是给开发人员看到，JavaScript 引擎会自动忽略：
//

1. // 这是一行注释
1. alert('hello'); // 这也是注释

另一种块注释是用 把多行字符包裹起来，把一大“块”视为一个注释：
/_..._/

| 1   | /\* 从这里开始是块注释 |
| --- | ---------------------- |
| 2   | 仍然是注释             |
| 3   | 仍然是注释             |
| 4   | 注释结束 \*/           |

请注意，JavaScript 严格区分大小写，如果弄错了大小写，程序将报错或者运行不正常。

## 、数据类型和变量

数据类型
计算机顾名思义就是可以做数学计算的机器，因此，计算机程序理所当然地可以处理各种数值。但是， 计算机能处理的远不止数值，还可以处理文本、图形、音频、视频、网页等各种各样的数据，不同的数 据，需要定义不同的数据类型。在 JavaScript 中定义了以下几种数据类型：
Number
JavaScript 不区分整数和浮点数，统一用 Number 表示，以下都是合法的 Number 类型：

| 1   | 123; // 整数 123                                                                           |
| --- | ------------------------------------------------------------------------------------------ |
| 2   | 0.456; // 浮点数 0.456                                                                     |
| 3   | 1.2345e3; // 科学计数法表示 1.2345x1000，等同于 1234.5                                     |
| 4   | -99; // 负数                                                                               |
| 5   | NaN; // NaN 表示 Not a Number，当无法计算结果时用 NaN 表示                                 |
| 6   | Infinity; // Infinity 表示无限大，当数值超过了 JavaScript 的 Number 所能表示的最大值时，就 |
|     | 表示为 Infinity                                                                            |

计算机由于使用二进制，所以，有时候用十六进制表示整数比较方便，十六进制用 0x 前缀和 0-9，a-f 表
0xff00
示，例如： ， ，等等，它们和十进制表示的数值完全一样。
0xa5b4c3d2
Number 可以直接做四则运算，规则和数学一致：

| 1   | 1 + 2; // 3          |     |
| --- | -------------------- | --- |
| 2   | (1 + 2) \* 5 / 2; // | 7.5 |
| 3   | 2 / 0; // Infinity   |     |
| 4   | 0 / 0; // NaN        |     |
| 5   | 10 % 3; // 1         |     |
| 6   | 10.5 % 3; // 1.5     |     |

注意字符串
%
是求余运算。

字符串是以单引号'或双引号"括起来的任意文本，比如 'abc' ， "xyz" 等等。请注
意， '' 或 "" 本身只是一种表示方式，不是字符串的一部分，因此，字符串 'abc' 只有
a ， b ， c 这 3 个 字 符 。
布尔值
布尔值和布尔代数的表示完全一致，一个布尔值只有 、 两种值，要么是 true ，要
true
false
么是 ，可以直接用 true 、 false 表示布尔值，也可以通过布尔运算计算出来：
false

| 1   | true; // 这是一个 true 值    |
| --- | ---------------------------- |
| 2   | false; // 这是一个 false 值  |
| 3   | 2 > 1; // 这是一个 true 值   |
| 4   | 2 >= 3; // 这是一个 false 值 |

运算是与运算，只有所有都为 ， 运算结果才是 ：
&&
true
&&
true

| 1   | true && true; // 这个&&语句计算结果为 true            |
| --- | ----------------------------------------------------- |
| 2   | true && false; // 这个&&语句计算结果为 false          |
| 3   | false && true && false; // 这个&&语句计算结果为 false |

运算是或运算，只要其中有一个为 ， 运算结果就是 ：
||
true
||
true

| 1   | false |     | false; // 这个 |     | 语句计算结果为 false |
| --- | ----- | --- | -------------- | --- | -------------------- | --- | ------------------- |
| 2   | true  |     | false; // 这个 |     | 语句计算结果为 true  |
| 3   | false |     | true           |     | false; // 这个       |     | 语句计算结果为 true |

运算是非运算，它是一个单目运算符，把 变成 ， 变成 ：
!
true
false
false
true

| 1   | !   | true; // 结果为 false   |
| --- | --- | ----------------------- |
| 2   | !   | false; // 结果为 true   |
| 3   | !   | (2 > 5); // 结果为 true |

布尔值经常用在条件判断中，比如：

1 var age = 15;
2 if (age >= 18) {

1. alert('adult');
1. } else {
1. alert('teenager'); 6 }

比较运算符
当我们对 Number 做比较时，可以通过比较运算符得到一个布尔值：

| 1   | 2   | > 5; // false |
| --- | --- | ------------- |
| 2   | 5   | >= 2; // true |
| 3   | 7   | == 7; // true |

实际上，JavaScript 允许对任意数据类型做比较：

| 1   | false | == 0; // true   |
| --- | ----- | --------------- |
| 2   | false | === 0; // false |

# 要特别注意相等运算符 。JavaScript 在设计时，有两种比较运算符：

# 第一种是 比较，它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果；

false

# 第二种是再比较。

比较，它不会自动转换数据类型，如果数据类型不一致，返回
，如果一致，

### 由于 JavaScript 这个设计缺陷，不要使用

==
**比较，始终坚持使用**
**比较。**
另一个例外是 这个特殊的 Number 与所有其他值都不相等，包括它自己：

===
NaN

| 1   | NaN | === | NaN; | //  | false |
| --- | --- | --- | ---- | --- | ----- |

唯一能判断
NaN
的方法是通过
函数：

1 isNaN(NaN); // true

最后要注意浮点数的相等比较：
isNaN()

| 1   | 1   | / 3 | === | (1  | -   | 2   | / 3); | //  | false |
| --- | --- | --- | --- | --- | --- | --- | ----- | --- | ----- |

这不是 JavaScript 的设计缺陷。浮点数在运算过程中会产生误差，因为计算机无法精确表示无限循环小 数。要比较两个浮点数是否相等，只能计算它们之差的绝对值，看是否小于某个阈值：

| 1   | Math.abs(1 | / 3 | -   | (1  | -   | 2   | / 3)) | <   | 0.0000001; | //  | true |
| --- | ---------- | --- | --- | --- | --- | --- | ----- | --- | ---------- | --- | ---- |

null 和 undeﬁned
表示一个“空”的值，它和 0 以及空字符串
null
''

不同，
0

是一个数值，
''

表示长度为 0

的字符串，而 表示“空”。
null
在其他语言中，也有类似 JavaScript 的 的表示，例如 Java 也用 null ，Swift 用 nil ，
null
Python 用 None 表示。但是，在 JavaScript 中，还有一个和 null 类似的 undefined ，它表示“未定义”。
JavaScript 的设计者希望用 表示一个空的值，而 undefined 表示值未定义。事实证明，这并
null

没有什么卵用，区分两者的意义不大。大多数情况下，我们都应该用 null 。断函数参数是否传递的情况下有用。
undefined
仅仅在判

数组
数组是一组按顺序排列的集合，集合的每个值称为元素。JavaScript 的数组可以包括任意数据类型。例 如：

| 1   | [1, | 2,  | 3.14, | 'Hello', | null, | true]; |
| --- | --- | --- | ----- | -------- | ----- | ------ |

上述数组包含 6 个元素。数组用另一种创建数组的方法是通过
[]
Array()
表示，元素之间用函数实现：
分隔。

| 1   | new | Array(1, | 2,  | 3); | //  | 创建了数组[1, | 2,  | 3]  |
| --- | --- | -------- | --- | --- | --- | ------------- | --- | --- |

然而，出于代码的可读性考虑，强烈建议直接使用 。
,
[]
数组的元素可以通过索引来访问。请注意，索引的起始值为 ：
0

| 1   | var arr | = [1, 2, 3.14, 'Hello', null,     | true]; |
| --- | ------- | --------------------------------- | ------ |
| 2   | arr[0]; | // 返回索引为 0 的元素，即 1      |        |
| 3   | arr[5]; | // 返回索引为 5 的元素，即 true   |        |
| 4   | arr[6]; | // 索引超出了范围，返回 undefined |        |

对象
JavaScript 的对象是一组由键-值组成的无序集合，例如：

1. var person = {
1. name: 'Bob',

3 age: 20,

1. tags: ['js', 'web', 'mobile'],
1. city: 'Beijing',
1. hasCar: true,
1. zipcode: null 8 };

JavaScript 对象的键都是字符串类型，值可以是任意数据类型。上述 person 对象一共定义了 6 个键值
对，其中每个键又称为对象的属性，例如， 的 属性为 'Bob' ， 属性
person
name
zipcode
为 。
null
要获取一个对象的属性，我们用 的方式：
对象变量.属性名

| 1   | person.name; // | 'Bob'   |
| --- | --------------- | ------- |
| 2   | person.zipcode; | // null |

变量
变量的概念基本上和初中代数的方程变量是一致的，只是在计算机程序中，变量不仅可以是数字，还可 以是任意数据类型。
变量在 JavaScript 中就是用一个变量名表示，变量名是大小写英文、数字、 $ 和 \_ 的组合，且不能

用数字开头。变量名也不能是 JavaScript 的关键字，如 、句，比如：
if
while
等。申明一个变量用 var 语

| 1   | var | a; // 申明了变量 a，此时 a 的值为 undefined            |
| --- | --- | ------------------------------------------------------ |
| 2   | var | $b = 1; // 申明了变量$b，同时给$b赋值，此时$b 的值为 1 |
| 3   | var | s_007 = '007'; // s_007 是一个字符串                   |
| 4   | var | Answer = true; // Answer 是一个布尔值 true             |
| 5   | var | t = null; // t 的值是 null                             |

变量名也可以用中文，但是，请不要给自己找麻烦。

在 JavaScript 中，使用等号 = 对变量进行赋值。可以把任意数据类型赋值给变量，同一个变量可以反
复赋值，而且可以是不同类型的变量，但是要注意只能用 申明一次，例如：
var

| 1   | var | a = 123; // a 的值是整数 123 |
| --- | --- | ---------------------------- |
| 2   | a = | 'ABC'; // a 变为字符串       |

这种变量本身类型不固定的语言称之为动态语言，与之对应的是静态语言。静态语言在定义变量时必须 指定变量类型，如果赋值的时候类型不匹配，就会报错。例如 Java 是静态语言，赋值语句如下：

| 1   | int | a = 123; // a 是整数类型变量，类型用 int 申明 |
| --- | --- | --------------------------------------------- |
| 2   | a = | "ABC"; // 错误：不能把字符串赋给整型变量      |

和静态语言相比，动态语言更灵活，就是这个原因。
请不要把赋值语句的等号等同于数学的等号。比如下面的代码：

| 1   | var | x   | =   | 10; |
| --- | --- | --- | --- | --- |
| 2   | x = | x   | +   | 2;  |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834437123-d4f52cb5-6987-434e-a5ec-f68fdfcfc43d.png#)如果从数学上理解 x = x + 2 那无论如何是不成立的，在程序中，赋值语句先计算右侧的表达式 x

- 2 ，得到结果 12 ，再赋给变量 x 。由于 x 之前的值是 10 ，重新赋值后， x 的值变成
  12 。
  打印变量 X，要显示变量的内容，可以用 ，打开 Chrome 的控制台就可以看到结果。
  console.log(x)

| 1   | var x = 100;    |
| --- | --------------- |
| 2   | console.log(x); |

使用 代替 的好处是可以避免弹出烦人的对话框。
console.log()
alert()

## 、strict 模式

JavaScript 在设计之初，为了方便初学者学习，并不强制要求用
var

申明变量。这个设计错误带来了

严重的后果：如果一个变量没有通过 申明就被使用，那么该变量就自动被申明为全局变量
var

| 1   | i   | =   | 10; | //  | i 现在是全局变量 |
| --- | --- | --- | --- | --- | ---------------- |

在同一个页面的不同的 JavaScript 文件中，如果都不用
var
申明，恰好都使用了变量
，将造成变

量 互相影响，产生难以调试的错误结果。
i
i
使用 申明的变量则不是全局变量，它的范围被限制在该变量被申明的函数体内（函数的概念将稍
var
后讲解），同名变量在不同的函数体内互不冲突。
为了修补 JavaScript 这一严重设计缺陷，ECMA 在后续规范中推出了 strict 模式，在 strict 模式下运行的
JavaScript 代码，强制通过 申明变量，未使用 var 申明变量就使用的，将导致运行错误。
var
启用 strict 模式的方法是在 JavaScript 代码的第一行写上：

1 'use strict';

这是一个字符串，不支持 strict 模式的浏览器会把它当做一个字符串语句执行，支持 strict 模式的浏览器将开启 strict 模式运行 JavaScript。
测试一下你的浏览器是否能支持 strict 模式：

| 1   | 'use strict';                                                      |
| --- | ------------------------------------------------------------------ |
| 2   | // 如果浏览器支持 strict 模式，下面的代码将报 ReferenceError 错误: |
| 3   | abc = 'Hello, world';                                              |
| 4   | console.log(abc);                                                  |

运行代码，如果浏览器报错，请修复后再运行。如果浏览器不报错，说明你的浏览器太古老了，需要尽 快升级。
不用 申明的变量会被视为全局变量，为了避免这一缺陷，所有的 JavaScript 代码都应该使用 strict
var
模式。我们在后面编写的 JavaScript 代码将全部采用 strict 模式。

## 、字符串

JavaScript 的字符串就是用 或
''
""

括起来的字符表示。

如果 ' 本身也是一个字符，那就可以用 "" 括起来，比如
"I'm OK"
I ， ' ， m ， 空 格 ， O ， K 这 6 个 字 符 。
"
\
包含的字符是

如果字符串内部既包含
'
又包含
怎么办？可以用转义字符
来标识，比如：

1 'I\'m \"OK\"!';

表示的字符串内容是：
I'm "OK"!
转义字符 \ 可以转义很多字符，比如所以 \\ 表示的字符就是 \ 。
\n

表示换行，

表示制表符，字符
\

本身也要转义，

ASCII 字符可以以 形式的十六进制表示，例如：
\t
\x##

| 1   | '\x41'; | //  | 完全等同于 | 'A' |
| --- | ------- | --- | ---------- | --- |

还可以用 表示一个 Unicode 字符：
\u####

| 1   | '\u4e2d\u6587'; | //  | 完全等同于 | '中文' |
| --- | --------------- | --- | ---------- | ------ |

多行字符串
由于多行字符串用引号 ``表示：
\n

写起来比较费事，所以最新的 ES6 标准新增了一种多行字符串的表示方法，用反

| 1   | `这是一个 |
| --- | --------- |
| 2   | 多行      |
| 3   | 字符串`;  |

注意：反引号在键盘的 ESC 下方，数字键 1 的左边：

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834437334-73b6667c-44d3-4a63-8927-5600fe779d14.png#)

模板字符串
要把多个字符串连接起来，可以用

-

号连接：

| 1   | var name = '小明';   |     |     |      |     |     |         |     |     |     |          |
| --- | -------------------- | --- | --- | ---- | --- | --- | ------- | --- | --- | --- | -------- |
| 2   | var age = 20;        |     |     |      |     |     |         |     |     |     |          |
| 3   | var message = '你好, | '   | +   | name | +   | ',  | 你今年' | +   | age | +   | '岁了!'; |
| 4   | alert(message);      |     |     |      |     |     |         |     |     |     |          |

如果有很多变量需要连接，用 号就比较麻烦。ES6 新增了一种模板字符串，表示方法和上面的多行

- 字符串一样，但是它会自动替换字符串中的变量：

| 1   | var name = '小明';   |          |                     |
| --- | -------------------- | -------- | ------------------- |
| 2   | var age = 20;        |          |                     |
| 3   | var message = `你好, | ${name}, | 你今年${age}岁了!`; |
| 4   | alert(message);      |          |                     |

操作字符串
字符串常见的操作如下：

| 1   | var s = 'Hello, world!'; |
| --- | ------------------------ |
| 2   | s.length; // 13          |

要获取字符串某个指定位置的字符，使用类似 Array 的下标操作，索引号从 0 开始：

| 1   | var s = 'Hello, world!';                                         |
| --- | ---------------------------------------------------------------- |
| 2   |                                                                  |
| 3   | s[0]; // 'H'                                                     |
| 4   | s[6]; // ' '                                                     |
| 5   | s[7]; // 'w'                                                     |
| 6   | s[12]; // '!'                                                    |
| 7   | s[13]; // undefined 超出范围的索引不会报错，但一律返回 undefined |

需要特别注意的是，字符串是不可变的，如果对字符串的某个索引赋值，不会有任何错误，但是，也没 有任何效果：

| 1   | var s = 'Test';             |
| --- | --------------------------- |
| 2   | s[0] = 'X';                 |
| 3   | alert(s); // s 仍然为'Test' |

JavaScript 为字符串提供了一些常用方法，注意，调用这些方法本身不会改变原有字符串的内容，而是返 回一个新字符串：

把一个字符串全部变为大写
toUpperCase()

1. var s = 'Hello';
1. s.toUpperCase(); // 返回'HELLO'

把一个字符串全部变为小写
toLowerCase()

1. var s = 'Hello';
1. var lower = s.toLowerCase(); // 返回'hello'并赋值给变量 lower
1. lower; // 'hello'

会搜索指定字符串出现的位置
indexOf()

1. var s = 'hello, world';
1. s.indexOf('world'); // 返回 7
1. s.indexOf('World'); // 没有找到指定的子串，返回-1

返回指定索引区间的子串
substring()

1. var s = 'hello, world'
1. s.substring(0, 5); // 从索引 0 开始到 5（不包括 5），返回'hello'
1. s.substring(7); // 从索引 7 开始到结束，返回'world'

## 、数组

JavaScript 的
Array

可以包含任意数据类型，并通过索引来访问每个元素。
length

要取得
Array
Array
的长度，直接访问
属性：

| 1   | var arr = [1, 2, 3.14, 'Hello', null, true]; |
| --- | -------------------------------------------- |
| 2   | arr.length; // 6                             |

请注意，直接给 的
Array
length
赋一个新的值会导致
大小的变化：

| 1   | var arr = [1, 2, 3];                                       |
| --- | ---------------------------------------------------------- |
| 2   | arr.length; // 3                                           |
| 3   | arr.length = 6;                                            |
| 4   | arr; // arr 变为[1, 2, 3, undefined, undefined, undefined] |
| 5   | arr.length = 2;                                            |
| 6   | arr; // arr 变为[1, 2]                                     |

Array 可以通过索引把对应的元素修改为新的值，因此，对个 Array
Array
的索引进行赋值会直接修改这

| 1   | var arr = ['A', 'B', 'C'];    |      |
| --- | ----------------------------- | ---- |
| 2   | arr[1] = 99;                  |      |
| 3   | arr; // arr 现在变为['A', 99, | 'C'] |

请注意，如果通过索引赋值时，索引超过了范围，同样会引起 大小的变化：
Array

| 1   | var arr = [1, 2, 3];   |     |            |            |      |
| --- | ---------------------- | --- | ---------- | ---------- | ---- |
| 2   | arr[5] = 'x';          |     |            |            |      |
| 3   | arr; // arr 变为[1, 2, | 3,  | undefined, | undefined, | 'x'] |

大多数其他编程语言不允许直接改变数组的大小，越界访问索引会报错。然而，JavaScript 的
Array

却不会有任何错误。在编写代码时，不建议直接修改界。
Array
的大小，访问索引时要确保索引不会越

常用方法

### indexOf

与 String 类似，
Array

也可以通过
indexOf()

来搜索一个指定的元素的位置：

| 1   | var arr = [10, 20, '30', 'xyz'];             |
| --- | -------------------------------------------- |
| 2   | arr.indexOf(10); // 元素 10 的索引为 0       |
| 3   | arr.indexOf(20); // 元素 20 的索引为 1       |
| 4   | arr.indexOf(30); // 元素 30 没有找到，返回-1 |
| 5   | arr.indexOf('30'); // 元素'30'的索引为 2     |

注意了，数字
30
Array
和字符串
是不同的元素。

### slice

'30'
slice() 就是对应 String 的新 的 Array
substring()

版本，它截取

的部分元素，然后返回一个

| 1   | var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];                    |       |      |      |
| --- | ----------------------------------------------------------------- | ----- | ---- | ---- |
| 2   | arr.slice(0, 3); // 从索引 0 开始，到索引 3 结束，但不包括索引 3: | ['A', | 'B', | 'C'] |
| 3   | arr.slice(3); // 从索引 3 开始到结束: ['D', 'E', 'F', 'G']        |       |      |      |

注意到 的起止参数包括开始索引，不包括结束索引。
slice()
如果不给 slice() 传递任何参数，它就会从头到尾截取所有元素。利用这一点，我们可以很容易地复制一个 Array ：

| 1   | var arr = | ['A', 'B', 'C', | 'D', | 'E', | 'F', | 'G']; |
| --- | --------- | --------------- | ---- | ---- | ---- | ----- |
| 2   | var aCopy | = arr.slice();  |      |      |      |       |
| 3   | aCopy; // | ['A', 'B', 'C', | 'D', | 'E', | 'F', | 'G']  |
| 4   | aCopy === | arr; // false   |      |      |      |       |

### push 和 pop

向
push()
Array

的末尾添加若干元素，
pop()

则把 的最后一个元素删除掉
Array

| 1   | var arr = [1, 2];                                         |
| --- | --------------------------------------------------------- |
| 2   | arr.push('A', 'B'); // 返回 Array 新的长度: 4             |
| 3   | arr; // [1, 2, 'A', 'B']                                  |
| 4   | arr.pop(); // pop()返回'B'                                |
| 5   | arr; // [1, 2, 'A']                                       |
| 6   | arr.pop(); arr.pop(); arr.pop(); // 连续 pop 3 次         |
| 7   | arr; // []                                                |
| 8   | arr.pop(); // 空数组继续 pop 不会报错，而是返回 undefined |
| 9   | arr; // []                                                |

### unshift 和 shift

如果要往 Array 的头部添加若干元素，使用
unshift()
shift()
Array 的第一个元素删掉：
方法，
方法则把

| 1   | var arr = [1, 2];                                             |
| --- | ------------------------------------------------------------- |
| 2   | arr.unshift('A', 'B'); // 返回 Array 新的长度: 4              |
| 3   | arr; // ['A', 'B', 1, 2]                                      |
| 4   | arr.shift(); // 'A'                                           |
| 5   | arr; // ['B', 1, 2]                                           |
| 6   | arr.shift(); arr.shift(); arr.shift(); // 连续 shift 3 次     |
| 7   | arr; // []                                                    |
| 8   | arr.shift(); // 空数组继续 shift 不会报错，而是返回 undefined |
| 9   | arr; // []                                                    |

### sort

可以对当前 Array 进行排序，它会直接修改当前
sort()
Array

的元素位置，直接调用

时，按照默认顺序排序

| 1   | var arr = ['B', 'C', 'A']; |
| --- | -------------------------- |
| 2   | arr.sort();                |
| 3   | arr; // ['A', 'B', 'C']    |

能否按照我们自己指定的顺序排序呢？完全可以，我们将在后面的函数中讲到。

### reverse

把整个
reverse()
Array

的元素给掉个个，也就是反转

| 1   | var arr = ['one', | 'two', | 'three']; |
| --- | ----------------- | ------ | --------- |
| 2   | arr.reverse();    |        |           |
| 3   | arr; // ['three', | 'two', | 'one']    |

### splice

方法是修改
splice()
Array

的“万能方法”，它可以从指定的索引开始删除若干元素，然后再

从该位置添加若干元素

| 1   | var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];      |
| --- | -------------------------------------------------------------------------- |
| 2   | // 从索引 2 开始删除 3 个元素,然后再添加两个元素:                          |
| 3   | arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', |
|     | 'Excite']                                                                  |
| 4   | arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']             |
| 5   | // 只删除,不添加:                                                          |
| 6   | arr.splice(2, 2); // ['Google', 'Facebook']                                |
| 7   | arr; // ['Microsoft', 'Apple', 'Oracle']                                   |
| 8   | // 只添加,不删除:                                                          |
| 9   | arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素     |
| 10  | arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']             |

### concat

方法把当前的
concat()
Array

和另一个
Array

连接起来，并返回一个新的
Array

| 1   | var arr = ['A', 'B', 'C'];         |
| --- | ---------------------------------- |
| 2   | var added = arr.concat([1, 2, 3]); |
| 3   | added; // ['A', 'B', 'C', 1, 2, 3] |
| 4   | arr; // ['A', 'B', 'C']            |

请注意，
concat()
Array
方法并没有修改当前
，而是返回了一个新的 。

实际上， concat() 方法可以接收任意个元素和部添加到新的 Array 里：
Array
Array
，并且自动把
拆开，然后全

| 1   | var arr = ['A', 'B', | 'C'];   |       |      |      |     |     |     |     |
| --- | -------------------- | ------- | ----- | ---- | ---- | --- | --- | --- | --- |
| 2   | arr.concat(1, 2, [3, | 4]); // | ['A', | 'B', | 'C', | 1,  | 2,  | 3,  | 4]  |

### join

方法是一个非常实用的方法，它把当前
join()
Array

的每个元素都用指定的字符串连接起

来，然后返回连接后的字符串：

| 1   | var arr = ['A', 'B', 'C', 1, 2, 3]; |
| --- | ----------------------------------- |
| 2   | arr.join('-'); // 'A-B-C-1-2-3'     |

如果 的元素不是字符串，将自动转换为字符串后再连接。
Array

### 多维数组

如果数组的某个元素又是一个
Array

，则可以形成多维数组，例如：

Array

| 1   | var | arr | =   | [[1, | 2,  | 3], | [400, | 500, | 600], | '-']; |
| --- | --- | --- | --- | ---- | --- | --- | ----- | ---- | ----- | ----- |

上述 包含 3 个元素，其中头两个元素本身也是 。
Array
Array
练习：如何通过索引取到 这个值：
500

| 1   | var x = arr[1][2];              |
| --- | ------------------------------- |
| 2   | console.log(x); // x 应该为 500 |

## 、对象

JavaScript 的对象是一种无序的集合数据类型，它由若干键值对组成。
JavaScript 的对象用于描述现实世界中的某个对象。

1. var person = {
1. name: '小明',

3 birth: 1990,
4 school: 'No.1 Middle School', 5 height: 1.70,

1. weight: 65,
1. score: null 8 }

1、定义一个对象

| 1 | var | 对象名 | = {
'value',
'value', 'value' |
| --- | --- | --- | --- |
| 2 | | key: | |
| 3 | | key: | |
| 4 | | key: | |
| 5 | } | | |

2、获取对象的属性

1 对象.属性

3、由于 JavaScript 的对象是动态类型，你可以自由地给一个对象添加或删除属性：

1. var xiaoming = {
1. name: '小明' 3 };
1. xiaoming.age; // undefined
1. xiaoming.age = 18; // 新增一个 age 属性
1. xiaoming.age; // 18
1. delete xiaoming.age; // 删除 age 属性
1. xiaoming.age; // undefined
1. delete xiaoming['name']; // 删除 name 属性
1. xiaoming.name; // undefined
1. delete xiaoming.school; // 删除一个不存在的 school 属性也不会报错

如果我们要检测对象是否拥有某一属性，可以用 in 操作符：

1. var xiaoming = {
1. name: '小明',

3 birth: 1990,
4 school: 'No.1 Middle School', 5 height: 1.70,

1. weight: 65,
1. score: null 8 };
1. 'name' in xiaoming; // true
1. 'grade' in xiaoming; // false

不过要小心，如果用 in 判断一个属性存在，这个属性不一定是 这个对象的，它可能是这个对象继承得到的：

| 1   | 'toString' | in  | xiaoming; | //  | true |
| --- | ---------- | --- | --------- | --- | ---- |

因为 toString 定义在 object 对象中，而所有对象最终都会在原型链上指向 object，所以 xiaoming 也拥有 toString 属性。
要判断一个属性是否是 xiaoming 自身拥有的，而不是继承得到的，可以用 hasOwnProperty() 方法：

1. var xiaoming = {
1. name: '小明' 3 };
1. xiaoming.hasOwnProperty('name'); // true
1. xiaoming.hasOwnProperty('toString'); // false

## 、流程控制

if 判断

1 var age = 3;
2 if (age >= 18) {

1. alert('adult');
1. } else if (age >= 6) {
1. alert('teenager');
1. } else {
1. alert('kid'); 8 }

for 循环

基础语法

| 1   | var x = 0;                 |
| --- | -------------------------- |
| 2   | var i;                     |
| 3   | for (i=1; i<=10000; i++) { |
| 4   | x = x + i;                 |
| 5   | }                          |
| 6   | x; // 50005000             |

遍历数组

1. var arr = ['Apple', 'Google', 'Microsoft'];
1. var i, x;
1. for (i=0; i<arr.length; i++) {
1. x = arr[i];
1. console.log(x); 6 }

无限循环

1. var x = 0;
1. for (;;) { // 将无限循环下去

3 if (x > 100) {
4 break; // 通过 if 判断来退出循环
5 }
6 x ++;
7 }

for ... in ， 它可以把一个对象的所有属性依次循环出来：

1. var o = {
1. name: 'Jack',

3 age: 20,
4 city: 'Beijing' 5 };

1. for (var key in o) {
1. if (o.hasOwnProperty(key)) {
1. console.log(key); // 'name', 'age', 'city' 9 }

10 }

由于 Array 也是对象，而它的每个元素的索引被视为对象的属性，所以遍历出来是下标

1 var a = ['A', 'B', 'C'];
2 for (var i in a) {
3 console.log(i); // '0', '1', '2'
4 console.log(a[i]); // 'A', 'B', 'C' 5 }

请注意，
for ... in
对 的循环得到的是
而不是 。

while 循环
基本操作
Array
String
Number

| 1 | var x var n while
x n
}
x; // | = 0; | |
| --- | --- | --- | --- |
| 2 | | = 99; | |
| 3 | | (n > 0) | { |
| 4 | | = x + n; | |
| 5 | | = n - 2; | |
| 6 | | | |
| 7 | | 2500 | |

do. while

1. var n = 0;
1. do {

3 n = n + 1;
4 } while (n < 100);
5 n; // 100

在编写循环代码时，务必小心编写初始条件和判断条件，尤其是边界值。
特别注意 和 是不同的判断逻辑。
i < 100
i <= 100

## 、Map 和 Set

JavaScript 的默认对象表示方式 { } 可以视为其他语言中的 Map 或 Dictionary 的数据结构，即一组键值对。但是 JavaScript 的对象有个小问题，就是键必须是字符串。但实际上 Number 或者其他数据类型作为 键也是非常合理的。
为了解决这个问题，最新的 ES6 规范引入了新的数据类型 。
Map

Map

是一组键值对的结构，具有极快的查找速度。
Map
举个例子，假设要根据同学的名字查找对应的成绩，如果用
Array

实现，需要两个 ：
Array

| 1   | var | names = | ['Michael', 'Bob', | 'Tracy']; |
| --- | --- | ------- | ------------------ | --------- |
| 2   | var | scores  | = [95, 75, 85];    |           |

给定一个名字，要查找对应的成绩，就先要在 names 中找到对应的位置，再从 scores 取出对应的成绩，
Array 越长，耗时越长。

如果用 Map 实现，只需要一个“名字”-“成绩”的对照表，直接根据名字查找成绩，无论这个表有多大，查找速度都不会变慢。用 JavaScript 写一个 Map 如下：
Map
Map

| 1   | var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]); |
| --- | --------------------------------------------------------------- |
| 2   | m.get('Michael'); // 95                                         |

初始化
Map
需要一个二维数组，或者直接初始化一个空 。
具有以下方法：

| 1   | var m = new Map(); // 空 Map                |
| --- | ------------------------------------------- |
| 2   | m.set('Adam', 67); // 添加新的 key-value    |
| 3   | m.set('Bob', 59);                           |
| 4   | m.has('Adam'); // 是否存在 key 'Adam': true |
| 5   | m.get('Adam'); // 67                        |
| 6   | m.delete('Adam'); // 删除 key 'Adam'        |
| 7   | m.get('Adam'); // undefined                 |

由于一个 key 只能对应一个 value，所以，多次对一个 key 放入 value，后面的值会把前面的值冲掉：

| 1   | var m = new Map();   |
| --- | -------------------- |
| 2   | m.set('Adam', 67);   |
| 3   | m.set('Adam', 88);   |
| 4   | m.get('Adam'); // 88 |

Set

和
Set
Map
没有重复的 key。
类似，也是一组 key 的集合，但不存储 value。由于 key 不能重复，所以，在 中，

| 1   | var | s1  | =   | new | Set(); // 空 Set |     |       |     |     |
| --- | --- | --- | --- | --- | ---------------- | --- | ----- | --- | --- |
| 2   | var | s2  | =   | new | Set([1, 2, 3]);  | //  | 含 1, | 2,  | 3   |

重复元素在 中自动被过滤：
Set
Set

| 1   | var | s   | = new Set([1, | 2, 3, | 3,  | '3']); |
| --- | --- | --- | ------------- | ----- | --- | ------ |
| 2   | s;  | //  | Set {1, 2, 3, | "3"}  |     |        |

注意数字
3
和字符串
是不同的元素。

通过 方法可以添加元素到 中，可以重复添加，但不会有效果：
'3'
add(key)
Set

| 1   | s.add(4);                     |
| --- | ----------------------------- |
| 2   | s; // Set {1, 2, 3, 4}        |
| 3   | s.add(4);                     |
| 4   | s; // 仍然是 Set {1, 2, 3, 4} |

通过 方法可以删除元素：
delete(key)

| 1   | var s = new Set([1, | 2,  | 3]); |
| --- | ------------------- | --- | ---- |
| 2   | s; // Set {1, 2, 3} |     |      |
| 3   | s.delete(3);        |     |      |
| 4   | s; // Set {1, 2}    |     |      |

## 、Iterable

遍历 Array 可以采用下标循环，遍历 Map 和 Set 就无法使用下标。
为了统一集合类型，ES6 标准引入了新的 类型，Array，Map，Set 属于；
iterable
具有 类型的集合可以通过新的 循环来遍历。
iterable
for ... of

遍历集合
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
var a = ['A', 'B', 'C'];
var s = new Set(['A', 'B', 'C']);
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
for (var x of a) { // 遍历 Array
console.log(x);
}
for (var x of s) { // 遍历 Set console.log(x);
}
for (var x of m) { // 遍 历 Map console.log(x[0] + '=' + x[1]);
}

更好的方式是直接使用
iterable
内置的
方法，它接收一个函数，每次迭代就自动回调该

函数。以 为例
forEach
Array

1. a.forEach(function (element, index, array) {
1. // element: 指向当前元素的值
1. // index: 指向当前索引
1. // array: 指向 Array 对象本身
1. console.log(element + ', index = ' + index); 6 });

方法是 ES5.1 标准引入的，你需要测试浏览器是否支持。没有索引，因此回调函数的前两个参数都是元素本身：
1 var s = new Set(['A', 'B', 'C']);

1. s.forEach(function (element, sameElement, set) {
1. console.log(element); 4 });

forEach()
Set

的回调函数参数依次为 、 和 本身：
Map
value
key
map

1 var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);

1. m.forEach(function (value, key, map) {
1. console.log(value); 4 });

# 3、函数

## 、函数定义和调用

1、定义函数方式一

1 function abs(x) { 2 if (x >= 0) {

1. return x;
1. } else {
1. return -x; 6 }

7 }

一旦执行到如果没有
return
return
时，函数就执行完毕，并将结果返回。
语句，函数执行完毕后也会返回结果，只是结果为 。
undefined

2、第二种定义函数的方式如下

1 var abs = function (x) { 2 if (x >= 0) {

1. return x;
1. } else {
1. return -x; 6 }

7 };

在这种方式下， 是一个匿名函数，它没有函数名。但是，这个匿名函数赋值
function (x) { ... }

给了变量 abs ，所以，通过变量 abs 就可以调用该函数。
上述两种定义完全等价，注意第二种方式按照完整语法需要在函数体末尾加一个 束。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834437565-08f4755d-9492-4dad-b1e9-7c1949f9ee3a.png#)
;

，表示赋值语句结

3、调用函数
调用函数时，按顺序传入参数即可：

| 1   | abs(10); | //  | 返回 10 |
| --- | -------- | --- | ------- |
| 2   | abs(-9); | //  | 返回 9  |

由于 JavaScript 允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函 数内部并不需要这些参数：

| 1   | abs(10, | 'blablabla'); // | 返回 10 |     |        |
| --- | ------- | ---------------- | ------- | --- | ------ |
| 2   | abs(-9, | 'haha', 'hehe',  | null);  | //  | 返回 9 |

传入的参数比定义的少也没有问题：

1 abs(); // 返回 NaN

此时 函数的参数
abs(x)
x
将收到
，计算结果为 。

要避免收到 ，可以对参数进行检查：
undefined
NaN
undefined

1. function abs(x) {
1. if (typeof x !== 'number') {
1. throw 'Not a number'; 4 }

5 if (x >= 0) {

1. return x;
1. } else {
1. return -x; 9 }

10 }

3、arguments

JavaScript 还有一个免费赠送的关键字的调用者传入的所有参数。
arguments
，它只在函数内部起作用，并且永远指向当前函数

1. function foo(x) {
1. console.log('x = ' + x); // 10
1. for (var i=0; i<arguments.length; i++) {
1. console.log('arg ' + i + ' = ' + arguments[i]); // 10, 20, 30 5 }

6 }
7 foo(10, 20, 30);

利用
arguments
以拿到参数的值：
，你可以获得调用者传入的所有参数。也就是说，即使函数不定义任何参数，还是可

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
function abs() {
if (arguments.length === 0) { return 0;
}
var x = arguments[0]; return x >= 0 ? x : -x;
}
abs(); // 0
abs(10); // 10
abs(-9); // 9

实际上 最常用于判断传入参数的个数。你可能会看到这样的写法：
arguments

1 // foo(a[, b], c)

1. // 接收 2~3 个参数，b 是可选参数，如果只传 2 个参数，b 默认为 null：
1. function foo(a, b, c) {
1. if (arguments.length === 2) {
1. // 实际拿到的参数是 a 和 b，c 为 undefined
1. c = b; // 把 b 赋给 c
1. b = null; // b 变为默认值 8 }

9 // ...
10 }

rest 参数
由于 JavaScript 函数允许接收任意个参数，于是我们就不得不用 来获取所有参数：
arguments

1. function foo(a, b) {
1. var i, rest = [];
1. if (arguments.length > 2) {
1. for (i = 2; i<arguments.length; i++) {
1. rest.push(arguments[i]); 6 }

7 }

1. console.log('a = ' + a);
1. console.log('b = ' + b);
1. console.log(rest); 11 }

为了获取除了已定义参数 、 b 之外的参数，我们不得不用 arguments ，并且循环要从索引
a
开始以便排除前两个参数，这种写法很别扭，只是为了获得额外的 rest 参数，有没有更好的方
2
法？
ES6 标准引入了 rest 参数，上面的函数可以改写为：

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
function foo(a, b, ...rest) { console.log('a = ' + a); console.log('b = ' + b); console.log(rest);
}
foo(1, 2, 3, 4, 5);
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]

foo(1);
// 结果:
// a = 1
// b = undefined
// Array []

rest 参数只能写在最后，前面用 ... 标识，从运行结果可知，传入的参数先绑定 a 、 b ，多余的参数以数组形式交给变量 rest ，所以，不再需要 arguments 我们就获取了全部参数。

## 、变量作用域

变量的作用域
在 JavaScript 中，用 申明的变量实际上是有作用域的。
如果一个变量在函数体内部申明，则该变量的作用域为整个函数体，在函数体外不可引用该变量：
var

1
2
3
4
5
6
7
8
'use strict';
function foo() { var x = 1;
x = x + 1;
}

x = x + 2; // ReferenceError! 无法在函数体外引用变量 x

如果两个不同的函数各自申明了同一个变量，那么该变量只在各自的函数体内起作用。换句话说，不同 函数内部的同名变量互相独立，互不影响：

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
'use strict';
function foo() { var x = 1;
x = x + 1;
}

function bar() { var x = 'A';
x = x + 'B';
}

由于 JavaScript 的函数可以嵌套，此时，内部函数可以访问外部函数定义的变量，反过来则不行：

1
2
3
4
5
6
7
8
9
'use strict';
function foo() { var x = 1; function bar() {
var y = x + 1; // bar 可以访问 foo 的变量 x!
}
var z = y + 1; // ReferenceError! foo 不可以访问 bar 的变量 y!
}

如果内部函数和外部函数的变量名重名怎么办？来测试一下：

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
function foo() { var x = 1; function bar() {
var x = 'A';
console.log('x in bar() = ' + x); // 'A'
}
console.log('x in foo() = ' + x); // 1 bar();
}
foo();

这说明 JavaScript 的函数在查找变量时从自身函数定义开始，从“内”向“外”查找。如果内部函数定义了与 外部函数重名的变量，则内部函数的变量将“屏蔽”外部函数的变量。

变量提升

JavaScript 的函数定义有个特点，它会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部：

1
2
3
4
5
6
7
8
9
'use strict';
function foo() {
var x = 'Hello, ' + y; console.log(x);
var y = 'Bob';
}

foo();

结果：Hello, undeﬁned ， 说明 y 的值为 undeﬁned；
y

这正是因为 JavaScript 引擎自动提升了变量
y
的声明，但不会提升变量
的赋值。

对于上述 函数，JavaScript 引擎看到的代码相当于：
foo()

1. function foo() {
1. var y; // 提升变量 y 的申明，此时 y 为 undefined
1. var x = 'Hello, ' + y;
1. console.log(x);
1. y = 'Bob'; 6 }

由于 JavaScript 的这一怪异的“特性”，我们在函数内部定义变量时，请严格遵守“在函数内部首先申明所有 变量”这一规则。最常见的做法是用一个 var 申明函数内部用到的所有变量：

1. function foo() {
1. var
1. x = 1, // x 初始化为 1
1. y = x + 1, // y 初始化为 2
1. z, i; // z 和 i 为 undefined
1. // 其他语句:

7 for (i=0; i<100; i++) { 8 ...
9 }
10 }

全局作用域
不在任何函数内定义的变量就具有全局作用域。实际上，JavaScript 默认有一个全局对象 ，全
局作用域的变量实际上被绑定到 的一个属性：
window
window

| 1   | 'use strict';                              |
| --- | ------------------------------------------ |
| 2   |                                            |
| 3   | var course = 'Learn JavaScript';           |
| 4   | alert(course); // 'Learn JavaScript'       |
| 5   | alert(window.course); // 'Learn JavaScript |

因此，直接访问全局变量
course
和访问
是完全一样的。

因此，顶层函数的定义也被视为一个全局变量，并绑定到 对象：
window.course
window

1
2
3
4
5
6
7
8
'use strict';
function foo() { alert('foo');
}

foo(); // 直接调用 foo()
window.foo(); // 通过 window.foo()调用

进一步大胆地猜测，我们每次直接调用的
alert()
函数其实也是
的一个变量：

| 1   | 'use strict';                        |
| --- | ------------------------------------ |
| 2   |                                      |
| 3   | window.alert('调用 window.alert()'); |
| 4   | // 把 alert 保存到另一个变量:        |
| 5   | var old_alert = window.alert;        |
| 6   | // 给 alert 赋一个新函数:            |
| 7   | window.alert = function () {}        |
| 8   | alert('无法用 alert()显示了!');      |
| 9   | // 恢复 alert:                       |
| 10  | window.alert = old_alert;            |
| 11  | alert('又可以用 alert()了!');        |

这说明 JavaScript 实际上只有一个全局作用域。任何变量（函数也视为变量），如果没有在当前函数作用
域中找到，就会继续往上查找，最后如果在全局作用域中也没有找到，则报 错误。
ReferenceError

全局变量会绑定到 上，不同的 JavaScript 文件如果使用了相同的全局变量，或者定义了相同名
window
字的顶层函数，都会造成命名冲突，并且很难被发现。减少冲突的一个方法是把自己的所有变量和函数 全部绑定到一个全局变量中。例如：

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
// 唯一的全局变量 MYAPP:
var MYAPP = {};
// 其他变量:
MYAPP.name = 'myapp'; MYAPP.version = 1.0;

// 其他函数:
MYAPP.foo = function () { return 'foo';
};

把自己的代码全部放入唯一的名字空间 中，会大大减少全局变量冲突的可能。
MYAPP
许多著名的 JavaScript 库都是这么干的：jQuery，YUI，underscore 等等。

局部作用域

由于 JavaScript 的变量作用域实际上是函数内部，我们在用域的变量的：
window
for
循环等语句块中是无法定义具有局部作

1
2
3
4
5
6
7
8
'use strict';
function foo() {
for (var i=0; i<100; i++) {
//
}
i += 100; // 仍然可以引用变量 i
}

为了解决块级作用域，ES6 引入了新的关键字 ，用的变量：
let
let
var
替代 可以申明一个块级作用域

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
'use strict';
function foo() { var sum = 0;
for (let i=0; i<100; i++) { sum += i;
}
// SyntaxError: i += 1;
}

常量
由于 和 申明的是变量，如果要申明一个常量，在 ES6 之前是不行的，我们通常用全部大写
的变量来表示“这是一个常量，不要修改它的值”：
var
let
const
let

| 1   | var | PI  | =   | 3.14; |
| --- | --- | --- | --- | ----- |

ES6 标准引入了新的关键字
const
来定义常量，
与 都具有块级作用域：

| 1   | 'use strict';                             |
| --- | ----------------------------------------- |
| 2   |                                           |
| 3   | const PI = 3.14;                          |
| 4   | PI = 3; // 某些浏览器不报错，但是无效果！ |
| 5   | PI; // 3.14                               |

## 、方法

定义方法
在一个对象中绑定函数，称为这个对象的方法。在 JavaScript 中，对象的定义是这样的：

1. var xiaoming = {
1. name: '小明',
1. birth: 1990

4 };

但是，如果我们给
xiaoming
绑定一个函数，就可以做更多的事情。比如，写个
方法，返

回 的年龄：
age()
xiaoming

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
var xiaoming = { name: '小明', birth: 1990,
age: function () {
var y = new Date().getFullYear(); return y - this.birth;
}
};
xiaoming.age; // function xiaoming.age()
xiaoming.age(); // 今年调用是 25,明年调用就变成 26 了

绑定到对象上的函数称为方法，和普通函数也没啥区别，但是它在内部使用了一个 个东东是什么？
this
在一个方法内部， this 是一个特殊变量，它始终指向当前对象，也就是
xiaoming
xiaoming
birth
关键字，这

这个变量。所
以，
this.birth
让我们拆开写：
可以拿到
的 属性。

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
function getAge() {
var y = new Date().getFullYear(); return y - this.birth;
}
var xiaoming = { name: '小明', birth: 1990,
age: getAge
};

xiaoming.age(); // 25, 正常结果
getAge(); // NaN

单独调用函数
getAge()
this
怎么返回了
？请注意，我们已经进入到了 JavaScript 的一个大坑里。

JavaScript 的函数内部如果调用了答案是，视情况而定！
NaN
this
，那么这个
到底指向谁？

如果以对象的方法形式调用，比如
xiaoming.age()
，该函数的
指向被调用的对象，也就

是 ，这是符合我们预期的。
this
xiaoming
this
window

如果单独调用函数，比如
getAge()
，此时，该函数的
指向全局对象，也就是 。

apply
不过，我们还是可以控制 的指向的！
this
要指定函数的 this 指向哪个对象，可以用函数本身的 apply 方法，它接收两个参数，第一个参数就是需要绑定的 this 变量，第二个参数是 Array ，表示函数本身的参数。

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
function getAge() {
var y = new Date().getFullYear(); return y - this.birth;
}
var xiaoming = { name: '小明', birth: 1990,
age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this 指向 xiaoming, 参数为空

# 4、标准对象

## 、标准对象

在 JavaScript 的世界里，一切都是对象。
但是某些对象还是和其他对象不太一样。为了区分对象的类型，我们用型，它总是返回一个字符串：
typeof

操作符获取对象的类

| 1   | typeof | 123; // 'number'          |
| --- | ------ | ------------------------- |
| 2   | typeof | NaN; // 'number'          |
| 3   | typeof | 'str'; // 'string'        |
| 4   | typeof | true; // 'boolean'        |
| 5   | typeof | undefined; // 'undefined' |
| 6   | typeof | Math.abs; // 'function'   |
| 7   | typeof | null; // 'object'         |
| 8   | typeof | []; // 'object'           |
| 9   | typeof | {}; // 'object'           |

## 、Date

在 JavaScript 中，
Date

对象用来表示日期和时间。

要获取系统当前时间，用：

| 1   | var now = new Date();                                       |
| --- | ----------------------------------------------------------- |
| 2   | now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)             |
| 3   | now.getFullYear(); // 2015, 年份                            |
| 4   | now.getMonth(); // 5, 月份，注意月份范围是 0~11，5 表示六月 |
| 5   | now.getDate(); // 24, 表示 24 号                            |
| 6   | now.getDay(); // 3, 表示星期三                              |
| 7   | now.getHours(); // 19, 24 小时制                            |
| 8   | now.getMinutes(); // 49, 分钟                               |
| 9   | now.getSeconds(); // 22, 秒                                 |
| 10  | now.getMilliseconds(); // 875, 毫秒数                       |
| 11  | now.getTime(); // 1435146562875, 以 number 形式表示的时间戳 |

注意，当前时间是浏览器从本机操作系统获取的时间，所以不一定准确，因为用户可以把当前时间设定 为任何值。

### 时区

对象表示的时间总是按浏览器所在时区显示的，不过我们既可以显示本地时间，也可以显示调 整后的 UTC 时间：
Date

| 1   | var d = new Date(1435146562875);                                                     |
| --- | ------------------------------------------------------------------------------------ |
| 2   | d.toLocaleString(); // '2015/6/24 下午 7:49:22'，本地时间（北京时区+8:00），显示的字 |
|     | 符串与操作系统设定的格式有关                                                         |
| 3   | d.toUTCString(); // 'Wed, 24 Jun 2015 11:49:22 GMT'，UTC 时间，与本地时间相差 8 小时 |

那么在 JavaScript 中如何进行时区转换呢？实际上，只要我们传递的是一个
number
们就不用关心时区转换。任何浏览器都可以把一个时间戳正确转换为本地时间。
类型的时间戳，我

时间戳是个什么东西？时间戳是一个自增的整数，它表示从 1970 年 1 月 1 日零时整的 GMT 时区开始的那一刻，到现在的毫秒数。假设浏览器所在电脑的时间是准确的，那么世界上无论哪个时区的电脑，它们此 刻产生的时间戳数字都是一样的，所以，时间戳可以精确地表示一个时刻，并且与时区无关。
所以，我们只需要传递时间戳，或者把时间戳从数据库里读出来，再让 JavaScript 自动转换为当地时间就 可以了。
要获取当前时间戳，可以用：

1. if (Date.now) {
1. console.log(Date.now()); // 老版本 IE 没有 now()方法
1. } else {
1. console.log(new Date().getTime()); 5 }

## 、JSON

JSON( JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式，目前使用特别广泛。
采用完全独立于编程语言的**文本格式**来存储和表示数据。
简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。
易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。
在 JavaScript 语言中，一切都是对象。因此，任何 JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。看看他的要求和语法格式：
对象表示为键值对，数据由逗号分隔花括号保存对象
方括号保存数组
**JSON 键值对**是用来保存 JavaScript 对象的一种方式，和 JavaScript 对象的写法也大同小异，键/值对组合中的键名写在前面并用双引号 "" 包裹，使用冒号 : 分隔，然后紧接着值：

| 1   | {"name": "QinJiang"} |
| --- | -------------------- |
| 2   | {"age": "3"}         |
| 3   | {"sex": "男"}        |

很多人搞不清楚 JSON 和 JavaScript 对象的关系，甚至连谁是谁都不清楚。其实，可以这么理解：

JSON 是 JavaScript 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串。

1. var obj = {a: 'Hello', b: 'World'}; //这是一个对象，注意键名也是可以使用引号包裹的
1. var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串

### JSON 和 JavaScript 对象互转

要实现从 JSON 字符串转换为 JavaScript 对象，使用 JSON.parse() 方法：

| 1   | var obj  | = JSON.parse('{"a": "Hello", | "b": | "World"}'); |
| --- | -------- | ---------------------------- | ---- | ----------- |
| 2   | //结果是 | {a: 'Hello', b: 'World'}     |      |             |

要实现从 JavaScript 对象转换为 JSON 字符串，使用 JSON.stringify() 方法：

| 1   | var json = JSON.stringify({a: 'Hello', b: 'World'}); |
| --- | ---------------------------------------------------- |
| 2   | //结果是 '{"a": "Hello", "b": "World"}'              |

### 代码测试

1. 新建一个 json-1.html ， 编写测试内容

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
18
19
20
21
22
23
24
25
26
27

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>JSON_秦疆</title>
</head>
<body>
<script type="text/javascript">
//编写一个js的对象var user = {
name:"秦疆", age:3,
sex:"男"
};
//将js对象转换成json字符串
var str = JSON.stringify(user); console.log(str);

//将 json 字符串转换为 js 对象
var user2 = JSON.parse(str); console.log(user2.age,user2.name,user2.sex);

</script>

</body>
</html>

1. 在 IDEA 中使用浏览器打开，查看控制台输出！

# 5、面向对象编程

面向对象编程
JavaScript 的面向对象编程和大多数其他语言如 Java、C#的面向对象编程都不太一样。如果你熟悉 Java 或 C#，很好，你一定明白面向对象的两个基本概念：

1. 类：类是对象的类型模板，例如，定义 类来表示学生，类本身是一种类型，

Student
表示学生类型，但不表示任何具体的某个学生；
Student

1. 实例：实例是根据类创建的对象，例如，根据 类可以创建出 、

Student
xiaoming
xiaohong 、 xiaojun 等多个实例，每个实例表示一个具体的学生，他们全都属于
Student 类型。
所以，类和实例是大多数面向对象编程语言的基本概念。
不过，在 JavaScript 中，这个概念需要改一改。JavaScript 不区分类和实例的概念，而是通过原型
（prototype）来实现面向对象编程。
Student

原型是指当我们想要创建
xiaoming
么办？恰好有这么一个现成的对象：
这个具体的学生时，我们并没有一个
类型可用。那怎

1. var robot = {
1. name: 'Robot',
1. height: 1.6,
1. run: function () {
1. console.log(this.name + ' is running...'); 6 }

7 };

我们看这个 对象有名字，有身高，还会跑，有点像小明，干脆就根据它来“创建”小明得了！
robot
于是我们把它改名为 ，然后创建出 ：
Student
xiaoming

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
var Student = { name: 'Robot', height: 1.2,
run: function () {
console.log(this.name + ' is running...');
}
};
var xiaoming = { name: '小明'
};

xiaoming. proto = Student;

注意最后一行代码把
xiaoming
继承下来的：
Student
的原型指向了对象
，看上去
仿佛是从

| 1   | xiaoming.name; // '小明'              |
| --- | ------------------------------------- |
| 2   | xiaoming.run(); // 小明 is running... |

有自己的 name 属性，但并没有定义 run() 方法。不过，由于小明是从继承而来，只要 Student 有 run() 方法， xiaoming 也可以调用：
Student
xiaoming
xiaoming
Student
JavaScript 的原型链和 Java 的 Class 区别就在，它没有“Class”的概念，所有对象都是实例，所谓继承关系不过是把一个对象的原型指向另一个对象而已。

如果你把 的原型指向其他对象：
xiaoming

1
2
3
4
5
6
7
var Bird = {
fly: function () {
console.log(this.name + ' is flying...');
}
};
xiaoming. proto = Bird;

现在 已经无法 了，他已经变成了一只鸟：
xiaoming
run()
Student
Bird

| 1   | xiaoming.fly(); | //  | 小明 | is  | flying... |
| --- | --------------- | --- | ---- | --- | --------- |

在 JavaScrip 代码运行时期，你可以把
xiaoming
从 变成
，或者变成任何对象。

class 继承
在上面的章节中我们看到了 JavaScript 的对象模型是基于原型实现的，特点是简单，缺点是理解起来比传 统的类－实例模型要困难，最大的缺点是继承的实现需要编写大量代码，并且需要正确实现原型链。
有没有更简单的写法？有！
class

新的关键字
class
从 ES6 开始正式被引入到 JavaScript 中。
的目的就是让定义类更简单。

我们先回顾用函数实现 的方法：
Student

1
2
3
4
5
6
7
8
9
function Student(name) { this.name = name;
}
// 现在要给这个 Student 新增一个方法

Student.prototype.hello = function () { alert('Hello, ' + this.name + '!');
}

如果用新的
class
关键字来编写
，可以这样写：

Student

1
2
3
4
5
6
7
8
9
class Student { constructor(name) {
this.name = name;
}
hello() {
alert('Hello, ' + this.name + '!');
}
}

比较一下就可以发现， class 的定义包含了构造函数 constructor 和定义在原型对象上的函数 hello() （注意没有 function 关键字），这样就避免了 Student.prototype.hello = function () {...} 这样分散的代码。
最后，创建一个 对象代码和前面章节完全一样：
Student

| 1   | var xiaoming = new Student('小明'); |
| --- | ----------------------------------- |
| 2   | xiaoming.hello();                   |

class 继承
用 class 定义对象的另一个巨大的好处是继承更方便了。想一想我们从 Student 派生一个
需要编写的代码量。现在，原型继承的中间对象，原型对象的构造函数等等都不需 要考虑了，直接通过 extends 来实现：
PrimaryStudent

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
class PrimaryStudent extends Student { constructor(name, grade) {
super(name); // 记得用 super 调用父类的构造方法!
this.grade = grade;
}
myGrade() {
alert('I am at grade ' + this.grade);
}
}

# 6、操作 BOM

## 、浏览器

由于 JavaScript 的出现就是为了能在浏览器中运行，所以，浏览器自然是 JavaScript 开发者必须要关注的。
目前主流的浏览器分这么几种：
IE 6~11：国内用得最多的 IE 浏览器，历来对 W3C 标准支持差。从 IE10 开始支持 ES6 标准；
Chrome：Google 出品的基于 Webkit 内核浏览器，内置了非常强悍的 JavaScript 引擎——V8。由于
Chrome 一经安装就时刻保持自升级，所以不用管它的版本，最新版早就支持 ES6 了；
Safari：Apple 的 Mac 系统自带的基于 Webkit 内核的浏览器，从 OS X 10.7 Lion 自带的 6.1 版本开始支持 ES6，目前最新的 OS X 10.11 El Capitan 自带的 Safari 版本是 9.x，早已支持 ES6； Firefox：Mozilla 自己研制的 Gecko 内核和 JavaScript 引擎 OdinMonkey。早期的 Firefox 按版本发 布，后来终于聪明地学习 Chrome 的做法进行自升级，时刻保持最新；
移动设备上目前 iOS 和 Android 两大阵营分别主要使用 Apple 的 Safari 和 Google 的 Chrome，由于两者都是 Webkit 核心，结果 HTML5 首先在手机上全面普及（桌面绝对是 Microsoft 拖了后腿），对
JavaScript 的标准支持也很好，最新版本均支持 ES6。
其他浏览器如 Opera 等由于市场份额太小就被自动忽略了。
另外还要注意识别各种国产浏览器，如某某安全浏览器，某某旋风浏览器，它们只是做了一个壳，其核 心调用的是 IE，也有号称同时支持 IE 和 Webkit 的“双核”浏览器。
不同的浏览器对 JavaScript 支持的差异主要是，有些 API 的接口不一样，比如 A JAX，File 接口。对于 ES6 标准，不同的浏览器对各个特性支持也不一样。
在编写 JavaScript 的时候，就要充分考虑到浏览器的差异，尽量让同一份 JavaScript 代码能运行在不同的浏览器中。
JavaScript 可以获取浏览器提供的很多对象，并进行操作。

## 、window

对象不但充当全局作用域，而且表示浏览器窗口。
window
对象有 和 属性，可以获取浏览器窗口的内部宽度和高度。
window
innerHeight
innerWidth
内部宽高是指除去菜单栏、工具栏、边框等占位元素后，用于显示网页的净宽高。
兼容性：IE<=8 不支持。

| 1   | 'use strict';                                                   |
| --- | --------------------------------------------------------------- |
| 2   |                                                                 |
| 3   | // 可以调整浏览器窗口大小试试:                                  |
| 4   | console.log('window inner size: ' + window.innerWidth + ' x ' + |
|     | window.innerHeight);                                            |

对应的，还有一个 和 属性，可以获取浏览器窗口的整个宽高。
outerWidth
outerHeight

## 、navigator

对象表示浏览器的信息，最常用的属性包括：
navigator
navigator.appName：浏览器名称； navigator.appVersion：浏览器版本； navigator.language：浏览器设置的语言； navigator.platform：操作系统类型；
navigator.userAgent：浏览器设定的 字符串。
User-Agent

| 1   | 'use strict';                                        |
| --- | ---------------------------------------------------- |
| 2   |                                                      |
| 3   | console.log('appName = ' + navigator.appName);       |
| 4   | console.log('appVersion = ' + navigator.appVersion); |
| 5   | console.log('language = ' + navigator.language);     |
| 6   | console.log('platform = ' + navigator.platform);     |
| 7   | console.log('userAgent = ' + navigator.userAgent);   |

请注意， 的信息可以很容易地被用户修改，所以 JavaScript 读取的值不一定是正确的。很
navigator
多初学者为了针对不同浏览器编写不同的代码，喜欢用 判断浏览器版本，例如：
if

1. var width;
1. if (getIEVersion(navigator.userAgent) < 9) {
1. width = document.body.clientWidth;
1. } else {
1. width = window.innerWidth; 6 }

但这样既可能判断不准确，也很难维护代码。正确的方法是充分利用 JavaScript 对不存在属性返回 的特性，直接用短路运算符 || 计算：
undefined

| 1   | var | width | =   | window.innerWidth |     |     |     | document.body.clientWidth; |
| --- | --- | ----- | --- | ----------------- | --- | --- | --- | -------------------------- |

## 、screen

对象表示屏幕的信息，常用的属性有：
screen
screen.width：屏幕宽度，以像素为单位； screen.height：屏幕高度，以像素为单位；
screen.colorDepth：返回颜色位数，如 8、16、24。

| 1   | console.log('Screen | size | =   | '   | +   | screen.width | +   | '   | x   | '   | +   | screen.height); |
| --- | ------------------- | ---- | --- | --- | --- | ------------ | --- | --- | --- | --- | --- | --------------- |

## 、location

对象表示当前页面的 URL 信息。例如，一个完整的 URL：
location

1 http://www.example.com:8080/path/index.html?a=1&b=2#TOP

可以用 获取。要获得 URL 各个部分的值，可以这么写：
location.href

| 1   | location.protocol; // 'http'             |
| --- | ---------------------------------------- |
| 2   | location.host; // 'www.example.com'      |
| 3   | location.port; // '8080'                 |
| 4   | location.pathname; // '/path/index.html' |
| 5   | location.search; // '?a=1&b=2'           |
| 6   | location.hash; // 'TOP'                  |

要加载一个新页面，可以调用
location.assign()
方法非常方便。
location.reload()
。如果要重新加载当前页面，调用

| 1   | location.reload();                                                        |
| --- | ------------------------------------------------------------------------- |
| 2   | location.assign('https://blog.kuangstudy.com/'); // 设置一个新的 URL 地址 |

## 、document

对象表示当前页面。由于 HTML 在浏览器中以 DOM 形式表示为树形结构， 对象就是整个 DOM 树的根节点。
document
document
的 属性是从 HTML 文档中的 读取的，但是可以动态改变：
document
title
xxx

1 document.title = '狂神说 Java!';

请观察浏览器窗口标题的变化。
要查找 DOM 树的某个节点，需要从我们先准备 HTML 数据：
document

对象开始查找。最常用的查找是根据 ID 和 Tag Name。

1. <dl id="code-menu" style="border:solid 1px #ccc;padding:6px;">
1. <dt>Java</dt>
1. <dd>Spring</dd>
1. <dt>Python</dt>
1. <dd>Django</dd>
1. <dt>Linux</dt>
1. <dd>Docker</dd> 8	</dl>

用 对象提供的 和 可以按 ID 获得一个
document
getElementById()
getElementsByTagName()
DOM 节点和按 Tag 名称获得一组 DOM 节点：

1
2
3
4
5
6
7
8
9
var menu = document.getElementById('code-menu'); var drinks = document.getElementsByTagName('dt');
var i, s;
s = '提供的饮料有:';
for (i=0; i<drinks.length; i++) {
s = s + drinks[i].innerHTML + ',';
}
console.log(s);

对象还有一个 属性，可以获取当前页面的 Cookie。
document
cookie
Cookie 是由服务器发送的 key-value 标示符。因为 HTTP 协议是无状态的，但是服务器要区分到底是哪个 用户发过来的请求，就可以用 Cookie 来区分。当一个用户成功登录后，服务器发送一个 Cookie 给浏览

器，例如
user=ABC123XYZ(加密的字符串)...
Cookie，服务器根据 Cookie 即可区分出用户。
，此后，浏览器访问该网站时，会在请求头附上这个

Cookie 还可以存储网站的一些设置，例如，页面显示的语言等等。
JavaScript 可以通过 读取到当前页面的 Cookie：
document.cookie

| 1   | document.cookie; | //  | 'v=123; | remember=true; | prefer=zh' |
| --- | ---------------- | --- | ------- | -------------- | ---------- |

由于 JavaScript 能读取到页面的 Cookie，而用户的登录信息通常也存在 Cookie 中，这就造成了巨大的安 全隐患，这是因为在 HTML 页面中引入第三方的 JavaScript 代码是允许的：

1. [<!--www.example.com--](http://www.example.com--/)>
1. <html>
1. <head>
1. <script src=["http://www.foo.com/jquery.js](http://www.foo.com/jquery.js)"></script>
1. </head>

6 ...
7 </html>

如果引入的第三方的 JavaScript 中存在恶意代码，则
[www.foo.com](http://www.foo.com/)
网站的用户登录信息。
[www.example.com](http://www.example.com/)
网站将直接获取到

为了解决这个问题，服务器在设置 Cookie 时可以使用 httpOnly ，设定了 httpOnly 的 Cookie 将不能被 JavaScript 读取。这个行为由浏览器实现，主流浏览器均支持 httpOnly 选项，IE 从 IE6 SP1 开始支持。
为了确保安全，服务器端在设置 Cookie 时，应该始终坚持使用 。
httpOnly

## 、history

history 对象保存了浏览器的历史记录，JavaScript 可以调用 history 对象的 或
back()
，相当于用户点击了浏览器的“后退”或“前进”按钮。
forward ()
这个对象属于历史遗留对象，对于现代 Web 页面来说，由于大量使用 A JAX 和页面交互，简单粗暴地调
用 可能会让用户感到非常愤怒。
history.back()

新手开始设计 Web 页面时喜欢在登录页登录成功时调用这是一种错误的方法。
history.back()
，试图回到登录前的页面。

任何情况，你都不应该使用 这个对象了。
history

# 7、操作 DOM

## 、选择器

由于 HTML 文档被浏览器解析后就是一棵 DOM 树，要改变 HTML 的结构，就需要通过 JavaScript 来操作
DOM。
始终记住 DOM 是一个树形结构。操作一个 DOM 节点实际上就是这么几个操作：
更新：更新该 DOM 节点的内容，相当于更新了该 DOM 节点表示的 HTML 的内容； 遍历：遍历该 DOM 节点下的子节点，以便进行进一步操作；
添加：在该 DOM 节点下新增一个子节点，相当于动态增加了一个 HTML 节点；
删除：将该节点从 HTML 中删除，相当于删掉了该 DOM 节点的内容以及它包含的所有子节点。
在操作一个 DOM 节点前，我们需要通过各种方式先拿到这个 DOM 节点。最常用的方法是
，以及 CSS 选择器
document.getElementById() 和 document.getElementsByTagName()
document.getElementsByClassName() 。

由于 ID 在 HTML 文档中是唯一的，所以 document.getElementById() 可以直接定位唯一的一个 DOM
节点。 和 document.getElementsByClassName() 总 是 返
document.getElementsByTagName()
回一组 DOM 节点。要精确地选择 DOM，可以先定位父节点，再从父节点开始选择，以缩小范围。
例如：

| 1   | // 返回 ID 为'test'的节点：                                                 |
| --- | --------------------------------------------------------------------------- |
| 2   | var test = document.getElementById('test');                                 |
| 3   |                                                                             |
| 4   | // 先定位 ID 为'test-table'的节点，再返回其内部所有 tr 节点：               |
| 5   | var trs = document.getElementById('test-table').getElementsByTagName('tr'); |
| 6   |                                                                             |
| 7   | // 先定位 ID 为'test-div'的节点，再返回其内部所有 class 包含 red 的节点：   |
| 8   | var reds = document.getElementById('test-                                   |
|     | div').getElementsByClassName('red');                                        |
| 9   |                                                                             |
| 10  | // 获取节点 test 下的所有直属子节点:                                        |
| 11  | var cs = test.children;                                                     |
| 12  |                                                                             |
| 13  | // 获取节点 test 下第一个、最后一个子节点：                                 |
| 14  | var first = test.firstElementChild;                                         |
| 15  | var last = test.lastElementChild;                                           |

第二种方法是使用 和
querySelector()
querySelectorAll()
用条件来获取节点，更加方便：
，需要了解 selector 语法，然后使

| 1   | // 通过 querySelector 获取 ID 为 q1 的节点：                  |
| --- | ------------------------------------------------------------- |
| 2   | var q1 = document.querySelector('#q1');                       |
| 3   |                                                               |
| 4   | // 通过 querySelectorAll 获取 q1 节点内的符合条件的所有节点： |
| 5   | var ps = q1.querySelectorAll('div.highlighted > p');          |

注意：低版本的 IE<8 不支持 和 。IE8 仅有限支持。
querySelector
querySelectorAll

## 、更新 DOM

拿到一个 DOM 节点后，我们可以对它进行更新。可以直接修改节点的文本，方法有两种：
一种是修改 属性，这个方式非常强大，不但可以修改一个 DOM 节点的文本内容，还可以
innerHTML
直接通过 HTML 片段修改 DOM 节点内部的子树：

| 1   | // 获取<p id="p-id">...</p>                           |       |
| --- | ----------------------------------------------------- | ----- |
| 2   | var p = document.getElementById('p-id');              |       |
| 3   | // 设置文本为 abc:                                    |       |
| 4   | p.innerHTML = 'ABC'; // <p id="p-id">ABC</p>          |       |
| 5   | // 设置 HTML:                                         |       |
| 6   | p.innerHTML = 'ABC <span style="color:red">RED</span> | XYZ'; |
| 7   | // <p>...</p>的内部结构已修改                         |       |

用 时要注意，是否需要写入 HTML。如果写入的字符串是通过网络拿到了，要注意对字符
innerHTML

编码来避免 XSS 攻击。
第二种是修改签：
innerText

属性，这样可以自动对字符串进行 HTML 编码，保证无法设置任何 HTML 标

| 1   | // 获取<p id="p-id">...</p>                      |
| --- | ------------------------------------------------ |
| 2   | var p = document.getElementById('p-id');         |
| 3   | // 设置文本:                                     |
| 4   | p.innerText = '<script>alert("Hi")</script>';    |
| 5   | // HTML 被自动编码，无法设置一个<script>节点:    |
| 6   | // <p id="p-id"><script>alert("Hi")</script></p> |

修改 CSS 也是经常需要的操作。DOM 节点的 属性对应所有的 CSS，可以直接获取或设置。因为
style
CSS 允许 font-size 这样的名称，但它并非 JavaScript 有效的属性名，所以需要在 JavaScript 中改写为驼峰式命名 fontSize ：

| 1   | // 获取<p id="p-id">...</p>              |
| --- | ---------------------------------------- |
| 2   | var p = document.getElementById('p-id'); |
| 3   | // 设置 CSS:                             |
| 4   | p.style.color = '#ff0000';               |
| 5   | p.style.fontSize = '20px';               |
| 6   | p.style.paddingTop = '2em';              |

## 、插入 DOM

appendChild

当我们获得了某个 DOM 节点，想在这个 DOM 节点内插入新的 DOM，应该如何做？

如果这个 DOM 节点是空的，例如，
`，那么，直接使用 内容，相当于“插入”了新的DOM节点。 如果这个DOM节点不是空的，那就不能这么做，因为有两个办法可以插入新的节点。一个是使用 innerHTML appendChild 子节点。例如： innerHTML = 'child'`就可以修改 DOM 节点的

会直接替换掉原来的所有子节点。
，把一个子节点添加到父节点的最后一个

1 <!-- HTML结构 -->

1. <p id="js">JavaScript</p>
1. <div id="list">
1. <p id="java">Java</p>
1. <p id="python">Python</p>
1. <p id="scheme">Scheme</p> 7	</div>

把 添加到的最后一项：
JavaScript

1
2
3
4
var
js = document.getElementById('js'), list = document.getElementById('list');
list.appendChild(js);

现在，HTML 结构变成了这样：

1 <!-- HTML结构 -->

1. <div id="list">
1. <p id="java">Java</p>
1. <p id="python">Python</p>
1. <p id="scheme">Scheme</p>
1. <p id="js">JavaScript</p> 7	</div>

因为我们插入的到新的位置。
js
节点已经存在于当前的文档树，因此这个节点首先会从原先的位置删除，再插入

更多的时候我们会从零创建一个新的节点，然后插入到指定位置：

1
2
3
4
5
6
var
list = document.getElementById('list'), haskell = document.createElement('p');
haskell.id = 'haskell'; haskell.innerText = 'Haskell';
list.appendChild(haskell);

这样我们就动态添加了一个新的节点：

1 <!-- HTML结构 -->

1. <div id="list">
1. <p id="java">Java</p>
1. <p id="python">Python</p>
1. <p id="scheme">Scheme</p>
1. <p id="haskell">Haskell</p> 7	</div>

动态创建一个节点然后添加到 DOM 树中，可以实现很多功能。举个例子，下面的代码动态创建了一个
节点，然后把它添加到 节点的末尾，这样就动态地给文档添加了新的 CSS 定义：

| 1   | var d = document.createElement('style');                 |
| --- | -------------------------------------------------------- |
| 2   | d.setAttribute('type', 'text/css');                      |
| 3   | d.innerHTML = 'p { color: red }';                        |
| 4   | //head 头部标签                                          |
| 5   | document.getElementsByTagName('head')[0].appendChild(d); |

可以在 Chrome 的控制台执行上述代码，观察页面样式的变化。

insertBefore
如果我们要把子节点插入到指定的位置怎么办？可以使用
，子节点会插入到
parentElement.insertBefore(newElement, referenceElement);
referenceElement 之前。
Python

还是以上面的 HTML 为例，假定我们要把
Haskell
插入到
之前：

1 <!-- HTML结构 -->

1. <div id="list">
1. <p id="java">Java</p>
1. <p id="python">Python</p>
1. <p id="scheme">Scheme</p> 6	</div>

可以这么写：

1
2
3
4
5
6
7
var
list = document.getElementById('list'), ref = document.getElementById('python'), haskell = document.createElement('p');
haskell.id = 'haskell';
haskell.innerText = 'Haskell'; list.insertBefore(haskell, ref);

新的 HTML 结构如下：

1 <!-- HTML结构 -->

1. <div id="list">
1. <p id="java">Java</p>
1. <p id="haskell">Haskell</p>
1. <p id="python">Python</p>
1. <p id="scheme">Scheme</p> 7	</div>

## 、删除 DOM

删除一个 DOM 节点就比插入要容易得多。
要删除一个节点，首先要获得该节点本身以及它的父节点，然后，调用父节点的 把自己删掉：
removeChild

| 1   | // 拿到待删除节点:                                   |
| --- | ---------------------------------------------------- |
| 2   | var self = document.getElementById('to-be-removed'); |
| 3   | // 拿到父节点:                                       |
| 4   | var parent = self.parentElement;                     |
| 5   | // 删除:                                             |
| 6   | var removed = parent.removeChild(self);              |
| 7   | removed === self; // true                            |

注意到删除后的节点虽然不在文档树中了，但其实它还在内存中，可以随时再次被添加到别的位置。

当你遍历一个父节点的子节点并进行删除操作时，要注意， 在子节点变化时会实时更新。
children
例如，对于如下 HTML 结构：
属性是一个只读属性，并且它

1. <div id="parent">
1. <p>First</p>
1. <p>Second</p> 4	</div>

当我们用如下代码删除子节点时：

| 1   | var parent = document.getElementById('parent');           |
| --- | --------------------------------------------------------- |
| 2   | parent.removeChild(parent.children[0]);                   |
| 3   | parent.removeChild(parent.children[1]); // <-- 浏览器报错 |

浏览器报错： parent.children[1] 不是一个有效的节点。原因就在于，当
First
后， parent.children 的节点数量已经从 2 变为了 1，索引 [1] 已经不存在了。
节点被删除

因此，删除多个节点时，要注意 属性时刻都在变化。
children

# 8、操作表单

## 、回顾

用 JavaScript 操作表单和操作 DOM 是类似的，因为表单本身也是 DOM 树。
不过表单的输入框、下拉框等可以接收用户输入，所以用 JavaScript 来操作表单，可以获得用户输入的内 容，或者对一个输入框设置新的内容。
HTML 表单的输入控件主要有以下几种：
文本框，对应的 <input type="text"> ，用于输入文本；
口令框，对应的 <input type="password"> ，用于输入口令； 单选框，对应的 <input type="radio"> ，用于选择一项；
复选框，对应的 <input type="checkbox"> ，用于选择多项； 下拉框，对应的 <select> ，用于选择一项；
隐藏文本，对应的服务器。
<input type="hidden">
，用户不可见，但表单提交时会把隐藏文本发送到

## 、获取值

如果我们获得了一个
<input>

节点的引用，就可以直接调用
value

获得对应的用户输入值：

| 1   | // <input type="text" id="email">             |
| --- | --------------------------------------------- |
| 2   | var input = document.getElementById('email'); |
| 3   | input.value; // '用户输入的值'                |

这种方式可以应用于 、
text
password
、 以及
。但是，对于单选框和复选

框， value 属性返回的永远是 HTML 预设的值，而我们需要获得的实际是用户是否“勾上了”选项，所以应该用 checked 判断：
hidden
select

| 1   | // <label><input type="radio" name="weekday" id="monday" value="1">  |
| --- | -------------------------------------------------------------------- |
|     | Monday</label>                                                       |
| 2   | // <label><input type="radio" name="weekday" id="tuesday" value="2"> |
|     | Tuesday</label>                                                      |
| 3   | var mon = document.getElementById('monday');                         |
| 4   | var tue = document.getElementById('tuesday');                        |
| 5   | mon.value; // '1'                                                    |
| 6   | tue.value; // '2'                                                    |
| 7   | mon.checked; // true 或者 false                                      |
| 8   | tue.checked; // true 或者 false                                      |

## 、设置值

设置值和获取值类似，对于 、就可以：
text
password

、 以及

，直接设置
hidden
value

| 1   | // <input type="text" id="email">                       |
| --- | ------------------------------------------------------- |
| 2   | var input = document.getElementById('email');           |
| 3   | input.value = 'test@example.com'; // 文本框的内容已更新 |

对于单选框和复选框，设置 为 或 即可。
select
checked
true
false

## 、提交表单

最后，JavaScript 可以以两种方式来处理表单的提交（A JAX 方式在后面章节介绍）。
方式一是通过 元素的 submit() 方法提交一个表单，例如，响应一个 的

<form>
button
事件，在JavaScript代码中提交表单：
click

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

<!-- HTML -->
<form id="test-form">
<input type="text" name="test">
<button type="button" onclick="doSubmitForm()">Submit</button>
</form>
<script>
function doSubmitForm() {
var form = document.getElementById('test-form');
// 可以在此修改form的input...
// 提 交 form: form.submit();
}
</script>

这种方式的缺点是扰乱了浏览器对 form 的正常提交。浏览器默认点击 时提交表单，或者用户在最后一个输入框按回车键。因此，第二种方式是响应 <form> 本身的
<button type="submit">
事件，在提交 form 时作修改：
onsubmit

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

<!-- HTML -->
<form id="test-form" onsubmit="return checkForm()">
<input type="text" name="test">
<button type="submit">Submit</button>
</form>
<script>
function checkForm() {
var form = document.getElementById('test-form');
// 可以在此修改form的input...
// 继续下一步: return true;
}
</script>

注意要 return true 来告诉浏览器继续提交，如果 ，浏览器将不会继续提交
return false
form，这种情况通常对应用户输入有误，提示用户错误信息后终止提交 form。
<input type="hidden">

在检查和修改
<input>
时，要充分利用
来传递数据。

例如，很多登录表单希望用户输入用户名和口令，但是，安全考虑，提交表单时不传输明文口令，而是
口令的 MD5。普通 JavaScript 开发人员会直接修改 ：
<input>

1
2
3
4
5
6
7
8

<!-- HTML -->
<form id="login-form" method="post" onsubmit="return checkForm()">
<input type="text" id="username" name="username">
<input type="password" id="password" name="password">
<button type="submit">Submit</button>
</form>
<script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.min.js">
</script>
9
10
11
12
13
14
15
16
17
18
<script>
function checkForm() {
var pwd = document.getElementById('password');
// 把用户输入的明文变为MD5: pwd.value = md5(pwd.value);
// 继续下一步: return true;
}
</script>

这个做法看上去没啥问题，但用户输入了口令提交时，口令框的显示会突然从几个

- （因为 MD5 有 32 个字符）。
- 变成 32 个

要想不改变用户的输入，可以利用 实现：
<input type="hidden">

| 1   | <script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.min.js"> |
| --- | ----------------------------------------------------------------------- |
|     | </script>                                                               |
| 2   |                                                                         |
| 3   | <!-- HTML -->                                                           |
| 4   | <form id="login-form" method="post" onsubmit="return checkForm()">      |

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
18
19
20
<input type="text" id="username" name="username">
<input type="password" id="input-password">
<input type="hidden" id="md5-password" name="password">
<button type="submit">Submit</button>

</form>
<script>
function checkForm() {
var input_pwd = document.getElementById('input-password'); var md5_pwd = document.getElementById('md5-password');
// 把用户输入的明文变为MD5:
md5_pwd.value = md5(input_pwd.value);
// 继续下一步: return true;
}
</script>

# 9、jQuery

## 、什么是 JQuery

你可能听说过 jQuery，它名字起得很土，但却是 JavaScript 世界中使用最广泛的一个库。
江湖传言，全世界大约有 80~90%的网站直接或间接地使用了 jQuery。鉴于它如此流行，又如此好用， 所以每一个入门 JavaScript 的前端工程师都应该了解和学习它。
jQuery 这么流行，肯定是因为它解决了一些很重要的问题。实际上，jQuery 能帮我们干这些事情：
消除浏览器差异：你不需要自己写冗长的代码来针对不同的浏览器来绑定事件，编写 A JAX 等代码；
document.getElementById('test')

简洁的操作 DOM 的方法：写
$('#test')
简洁；
轻松实现动画、修改 CSS 等各种操作。
肯定比 来得

jQuery 的理念“Write Less, Do More“，让你写更少的代码，完成更多的工作！ 官网：[https://jquery.com/](https://jquery.com/)
jQuery 只是一个 文件，但你会看到有 compressed（已压缩）和 uncompressed（未
jquery-xxx.js
压缩）两种版本，使用时完全一样，但如果你想深入研究 jQuery 源码，那就用 uncompressed 版本。
使用 jQuery 只需要在页面的 引入 jQuery 文件即可：
head

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

<html>
<head>
<script src="//code.jquery.com/jquery-1.11.3.min.js"></script>
...
</head>
<body>
<a id="test-link" href="#0">点我试试</a>
<script>
// 获取超链接的jQuery对象: var a = $('#test-link'); a.on('click', function () {
alert('Hello!');
});

1. // 方式二
1. a.click(function () {
1. alert('Hello!'); 18 });
1. </script>
1. </body>
1. </html>

公式：
$(selector).action()
美元符号定义 jQuery
选择符（selector）"查询"和"查找" HTML 元素
jQuery 的 action() 执行对元素的操作

## 、选择器

了解
选择器是 jQuery 的核心。
为什么 jQuery 要发明选择器？回顾一下 DOM 操作中我们经常使用的代码：

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
// 按 ID 查找：
var a = document.getElementById('dom-id');
// 按 tag 查找：
var divs = document.getElementsByTagName('div');

// 查找<p class="red">：
var ps = document.getElementsByTagName('p');
// 过滤出 class="red":
// TODO:

// 查找<table class="green">里面的所有<tr>： var table = ...
for (var i=0; i<table.children; i++) {
// TODO: 过滤出<tr>
}

这些代码实在太繁琐了！
jQuery 的选择器就是帮助我们快速定位到一个或多个 DOM 节点。

### 按 ID 查找

如果某个 DOM 节点有
id

属性，利用 jQuery 查找如下：

| 1   | // 查找<div id="abc">: |
| --- | ---------------------- |
| 2   | var div = $('#abc');   |

### 按 tag 查找

按 tag 查找只需要写上 tag 名称就可以了：

| 1   | var ps = $('p'); // 返回所有<p>节点     |
| --- | --------------------------------------- |
| 2   | ps.length; // 数一数页面有多少个<p>节点 |

### 按 class 查找

按 class 查找注意在 class 名称前加一个 ：
.

| 1   | var a = $('.red'); // 所有节点包含`class="red"`都将返回 |                              |
| --- | ------------------------------------------------------- | ---------------------------- |
| 2   | //                                                      | 例如:                        |
| 3   | //                                                      | <div class="red">...</div>   |
| 4   | //                                                      | <p class="green red">...</p> |

### 按属性查找

一个 DOM 节点除了 id 和个表单中按属性来查找：
class

外还可以有很多属性，很多时候按属性查找会非常方便，比如在一

| 1   | var | email = $('[name=email]'); // 找出<??? name="email">               |
| --- | --- | ------------------------------------------------------------------ |
| 2   | var | passwordInput = $('[type=password]'); // 找出<??? type="password"> |
| 3   | var | a = $('[items="A B"]'); // 找出<??? items="A B">                   |

当属性的值包含空格等特殊字符时，需要用双引号括起来。按属性查找还可以使用前缀查找或者后缀查找：

| 1   | var icons = $('[name^=icon]'); // 找出所有 name 属性值以 icon 开头的 DOM |
| --- | ------------------------------------------------------------------------ |
| 2   | // 例如: name="icon-1", name="icon-2"                                    |
| 3   | var names = $('[name$=with]'); // 找出所有 name 属性值以 with 结尾的 DOM |
| 4   | // 例如: name="startswith", name="endswith"                              |

## 、操作 DOM

修改 Text 和 HTML

jQuery 对象的 和结构：
text()
html()
方法分别获取节点的文本和原始 HTML 文本，例如，如下的 HTML

1 <!-- HTML结构 -->

1. <ul id="test-ul">
1. <li class="js">JavaScript</li>
1. <li name="book">Java & JavaScript</li> 5	</ul>

分别获取文本和 HTML：

| 1   | $('#test-ul | li[name=book]').text(); | //  | 'Java | & JavaScript' |
| --- | ----------- | ----------------------- | --- | ----- | ------------- |
| 2   | $('#test-ul | li[name=book]').html(); | //  | 'Java | & JavaScript' |

如何设置文本或 HTML？jQuery 的 API 设计非常巧妙：无参数调用 成设置文本，HTML 也是类似操作，自己动手试试：
text()
是获取文本，传入参数就变

| 1   | var j1 = $('#test-ul li.js');                          |
| --- | ------------------------------------------------------ |
| 2   | var j2 = $('#test-ul li[name=book]');                  |
| 3   | j1.html('<span style="color: red">JavaScript</span>'); |
| 4   | j2.text('JavaScript & ECMAScript');                    |

一个 jQuery 对象可以包含 0 个或任意个 DOM 对象，它的方法实际上会作用在对应的每个 DOM 节点上。在上面的例子中试试：

| 1   | $('#test-ul | li').text('JS'); | //  | 是不是两个节点都变成了 JS？ |
| --- | ----------- | ---------------- | --- | --------------------------- |

修改 CSS
jQuery 对象有“批量操作”的特点，这用于修改 CSS 实在是太方便了。考虑下面的 HTML 结构：

1 <!-- HTML结构 -->

1. <ul id="test-css">
1. <li class="lang dy"><span>JavaScript</span></li>
1. <li class="lang"><span>Java</span></li>
1. <li class="lang dy"><span>Python</span></li>
1. <li class="lang"><span>Swift</span></li>
1. <li class="lang dy"><span>Scheme</span></li> 8	</ul>

要高亮显示动态语言，调用 jQuery 对象的 方法，我们用一行语句实现：
css('name', 'value')

1 $('#test-css li.dy>span').css('background-color', '#ffd351').css('color', 'red');

注意，jQuery 对象的所有方法都返回一个 jQuery 对象（可能是新的也可能是自身），这样我们可以进行链式调用，非常方便。
jQuery 对象的 方法可以这么用：
css()
class

| 1   | var div = $('#test-div');                     |
| --- | --------------------------------------------- |
| 2   | div.css('color'); // '#000033', 获取 CSS 属性 |
| 3   | div.css('color', '#336699'); // 设置 CSS 属性 |
| 4   | div.css('color', ''); // 清除 CSS 属性        |

方法将作用于 DOM 节点的以用 jQuery 提供的下列方法：
css()
style
属性，具有最高优先级。如果要修改
属性，可

| 1   | var div = $('#test-div');     |                                  |
| --- | ----------------------------- | -------------------------------- |
| 2   | div.hasClass('highlight'); // | false， class 是否包含 highlight |
| 3   | div.addClass('highlight'); // | 添加 highlight 这个 class        |
| 4   | div.removeClass('highlight'); | // 删除 highlight 这个 class     |

显示和隐藏 DOM
要隐藏一个 DOM，我们可以设置 CSS 的 display 属性为 ，利用 css() 方法就可以实现。
不过，要显示这个 DOM 就需要恢复原有的 display 属性，这就得先记下来原有的 display 属性到
none
底是 还是 inline 还是别的值。
block
考虑到显示和隐藏 DOM 元素使用非常普遍，jQuery 直接提供 和 方法，我们不用关
show()
hide()
心它是如何修改 属性的，总之它能正常工作：
display

| 1   | var a = $('a[target=_blank]'); |
| --- | ------------------------------ |
| 2   | a.hide(); // 隐藏              |
| 3   | a.show(); // 显示              |

注意，隐藏 DOM 节点并未改变 DOM 树的结构，它只影响 DOM 节点的显示。这和删除 DOM 节点是不同的。

获取 DOM 信息
利用 jQuery 对象的若干方法，我们直接可以获取 DOM 的高宽等信息，而无需针对不同浏览器编写特定代码：

| 1   | // 浏览器可视窗口大小:                                                         |
| --- | ------------------------------------------------------------------------------ |
| 2   | $(window).width(); // 800                                                      |
| 3   | $(window).height(); // 600                                                     |
| 4   |                                                                                |
| 5   | // HTML 文档大小:                                                              |
| 6   | $(document).width(); // 800                                                    |
| 7   | $(document).height(); // 3500                                                  |
| 8   |                                                                                |
| 9   | // 某个 div 的大小:                                                            |
| 10  | var div = $('#test-div');                                                      |
| 11  | div.width(); // 600                                                            |
| 12  | div.height(); // 300                                                           |
| 13  | div.width(400); // 设置 CSS 属性 width: 400px，是否生效要看 CSS 是否有效       |
| 14  | div.height('200px'); // 设置 CSS 属性 height: 200px，是否生效要看 CSS 是否有效 |

和 方法用于操作 DOM 节点的属性：
attr()
removeAttr()

| 1   | // <div id="test-div" name="Test" start="1">...</div>     |
| --- | --------------------------------------------------------- |
| 2   | var div = $('#test-div');                                 |
| 3   | div.attr('data'); // undefined, 属性不存在                |
| 4   | div.attr('name'); // 'Test'                               |
| 5   | div.attr('name', 'Hello'); // div 的 name 属性变为'Hello' |
| 6   | div.removeAttr('name'); // 删除 name 属性                 |
| 7   | div.attr('name'); // undefined                            |

操作表单

对于表单元素，jQuery 对象统一提供
val()
方法获取和设置对应的
属性：

value

| 1 | /\* |
<input id="test-input" name="email" value="">
<select id="test-select" name="city">

<option value="BJ" selected>Beijing</option>
<option value="SH">Shanghai</option>
<option value="SZ">Shenzhen</option>
</select>
<textarea id="test-textarea">Hello</textarea>

| input = $('#test-input'), |
| ------------------------- | --- | --- |
| 2                         |     |     |
| 3                         |     |     |
| 4                         |     |     |
| 5                         |     |     |
| 6                         |     |     |
| 7                         |     |     |
| 8                         |     |     |
| 9                         | \*/ |     |
| 10                        | var |     |
| 11                        |     |     |

12
13
14
15
16
17
18
19
20
21
22
select = $('#test-select'),
textarea = $('#test-textarea');
input.val(); // 'test'
input.val('abc@example.com'); // [文本框的内容已变为 abc@example.com](mailto:文本框的内容已变为abc@example.com)

select.val(); // 'BJ' select.val('SH'); // 选择框已变为 Shanghai

textarea.val(); // 'Hello' textarea.val('Hi'); // 文本区域已更新为'Hi'

可见，一个 就统一了各种输入框的取值和赋值的问题。
val()
append()

添加 DOM

要添加新的 DOM 节点，除了通过 jQuery 的如：
html()
这种暴力方法外，还可以用
方法，例

1 <div id="test-div"> 2 <ul>

1. <li><span>JavaScript</span></li>
1. <li><span>Python</span></li>
1. <li><span>Swift</span></li> 6	</ul>

7 </div>

如何向列表新增一个语言？首先要拿到 节点：

<ul>

| 1   | var | ul  | =   | $('#test-div>ul'); |
| --- | --- | --- | --- | ------------------ |

然后，调用 传入 HTML 片段：
append()

1 ul.append('<li><span>Haskell</span></li>');

把 DOM 添加到最后， 则把 DOM 添加到最前。
append()
prepend()
如果要把新节点插入到指定位置，例如，JavaScript 和 Python 之间，那么，可以先定位到 JavaScript，然
after()
后用 方法：

| 1   | var js = $('#test-div>ul>li:first-child'); |
| --- | ------------------------------------------ |
| 2   | js.after('<li><span>Lua</span></li>');     |

也就是说，同级节点可以用 或者 方法。
after()
before()

删除节点

要删除 DOM 节点，拿到 jQuery 对象后直接调用
remove()
DOM 节点，实际上可以一次删除多个 DOM 节点：
方法就可以了。如果 jQuery 对象包含若干

| 1   | var li = $('#test-div>ul>li');   |
| --- | -------------------------------- |
| 2   | li.remove(); // 所有<li>全被删除 |

## 、事件

jQuery 能够绑定的事件主要包括：

### 鼠标事件

click: 鼠标单击时触发； dblclick：鼠标双击时触发； mouseenter：鼠标进入时触发； mouseleave： 鼠标移出时触发； mousemove：鼠标在 DOM 内部移动时触发； hover：鼠标进入和退出时触发两个函数，相当于 mouseenter 加上 mouseleave。

### 键盘事件

键盘事件仅作用在当前焦点的 DOM 上，通常是 。
和
keydown：键盘按下时触发； keyup：键盘松开时触发； keypress：按一次键后触发。

### 其他事件

focus：当 DOM 获得焦点时触发； blur：当 DOM 失去焦点时触发； change：当 、 或
的内容改变时
触发； submit：当 提交时触发； ready：当页面被载入并且 DOM 树完成初始化后触发。
ready

初始化事件

我们自己的初始化代码必须放到
document
对象的
事件中，保证 DOM 已完成初始化：

1. <html>
1. <head>
1. <script>
1. $(document).on('ready', function () {
1. $('#testForm).on('submit', function () {
1. alert('submit!');

7 });
8 });
9 </script>

1. </head>
1. <body>
1. <form id="testForm"> 13		...
1. </form>
1. </body>

这样写就没有问题了。因为相关代码会在 DOM 树初始化后再执行。
由于 事件使用非常普遍，所以可以这样简化：
ready

1. $(document).ready(function () {
1. // on('submit', function)也可以简化:
1. $('#testForm).submit(function () {
1. alert('submit!'); 5 });

6 });

甚至还可以再简化为：

1 $(function () { 2 // init... 3 });

事件参数
有些事件，如 和 keypress ，我们需要获取鼠标位置和按键的值，否则监听这些事件就
没什么意义了。所有事件都会传入 Event 对象作为参数，可以从 Event 对象上获取到更多的信息：
mousemove

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
18
19
20
21
22
23
24
25
26

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Title</title>
<style>
#testMouseMoveDiv{ width: 300px; height: 300px;
border: 1px solid black;
}
</style>
</head>
<body>
mousemove: <span id="testMouseMoveSpan"></span>

<div id="testMouseMoveDiv">
在此区域移动鼠标试试
</div>
27
28
29
30
31
32
<script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.js"></script>
<script>
$(function () {
$('#testMouseMoveDiv').mousemove(function (e) {
$('#testMouseMoveSpan').text('pageX = ' + e.pageX + ', pageY = '
+ e.pageY);
});
});
</script>
</body>
</html>
