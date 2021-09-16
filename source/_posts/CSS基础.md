---
title: CSS基础
categories: 
	- CSS
tags:
	- CSS
	- 基础知识
keywords: CSS 基础知识
mathjax: true
description:
    讲解CSS基础知识
date: 2019-02-12
cover: https://gitee.com/lastknightcoder/blogimage/raw/master/img/bg25.jpg
---

# CSS入门

之前我们学习过HTML的基本知识，对于网页来说，HTML搭建的只是框架，而对于网页的美容，则需要用到CSS，CSS就可以看做是网页的美容师。CSS的全称叫做Cascading Style Sheets,中文名字叫层叠样式表。我们可以用CSS设置字体的颜色，字体的大小以及图片外形，排版布局等等"美颜"的工作。

## CSS的书写位置

### 行内式
CSS的书写位置有三个，第一个是行内，如下

```html
<标签 style="属性1:属性值1; 属性2:属性值2 ...">
```
下面就是把这个一级标题设置为红色。

```html
<!DOCTYPE html>
<html>
    <head>

    </head>

    <body>
        <h1 style = "color:red">CSS入门</h1>
    </body>
</html>
```
### 内嵌式
内嵌式是将CSS代码集中写在HTML文档的head头部标签中，并且用style标签定义，其基本语法格式如下：

```html
<head>
    <style>
        选择器{属性1:属性值1; 属性2:属性值2; ...}
    </style>
</head>
```
比如下面
```html
<head>
    <style>
        h1{color:red;}
    </style>
</head>
```
就是把所有的h1标签的文字颜色设置为红色。

### 外链式
外链式指的是HTML文件与CSS文件分开写，在实际工作中就是这么干的。但是为了方便，我在介绍CSS的使用的时候一般会用内嵌式。外链式的写法如下

```html
<link rel="stylesheet" href="CSS文件的路径" type="text/CSS"/ >
```

link标签是单标签，它是放在head标签内的。在使用link标签时，必须指定link标签的三个属性,如下:

- href:定义所链接外部样式表文件的URL,可以是相对路径，也可以是绝对路径。
- type:定义所链接文档的类型，在这里需要指定为"text/CSS",表示链接的外部文件为CSS样式表。
- rel:定义当前文档与被链接文档之间的关系，在这里需要指定为"stylesheet"，表示被链接的文档是一个样式表文件。

## 选择器

### 标签选择器
使用HTML的标签名作为选择器，为HTML的某一标签指定统一的`CSS`样式。比如上面的

```css
h1{color:red;}
```
就是标签选择器，使用h1作为选择器，所有的h1标签包含的文字都被设置为红色。

### 类选择器

类选择器是最常用的选择器，类选择器使用.（英文点号）进行标识，后面紧跟类名，其基本语法格式如下：

```css
.类名{
    属性:属性值;
    ... ...
}
```
标签在调用CSS样式时，使class="类名"即可，比如

```html
<head>
    <style>
        .red{
            color : red;
        }
    </style>
</head>

<body>
    <h1 class="red">CSS入门</h1>
</body>
```

我们声明了一个类选择器，类名叫做red,在h1标签中，我们通过class="red"调用了该类，所以h1标签所包含的文字设置成了红色。

### 多类名选择器

如果我们在标签中需要调用多个类怎么办，比如有两个类分别为

```html
.red{
    color:red;
}
.font14{
    font-size=14px;
}
```

如果加入h1标签要调用这两个类的话，写法如

```html
<h1 class = "red font14">标题</h1>
```
这个就叫做多类名选择器。

还有一个问题就是，如果调用的多个类有冲突怎么办?比如调用的两个类一个设置颜色为红色，一个设置颜色为蓝色，是如何处理这种冲突?

```html
.red{
    color:red;
}
.blue{
    color:blue;
}
```
假设h1调用如下

```html
<h1 class="blue red">标题</h1>
```

你觉得会是蓝色还是红色的?答案是蓝色的，虽然调用时blue写在red前面，你想red会覆盖blue,所以会是红色，实际上最终的效果与调用的顺序无关，而与CSS书写的顺序有关，由于CSS书写时red在blue的上面，所以只有最后的blue生效。

### id选择器

与类选择器很相似，不管调用方式是通过id来调用的，并且书写方式上有差别，即将.改为了\#。如下

```css
#id名{
    属性:属性值;
    ... ...
}
```

调用方式同类调用方式，不过用的不是class=""而是id=""。与类选择器不同的是，由于id是唯一的，不同的标签的id不能相同，所以该种方法只能给唯一的一个id设置样式。并且id选择器能做到的，类选择器也能做到，所以实际上类选择器是用的最多的。

### 通配符选择器

书写方式如下
```css
*{
    属性:属性值;
    ... ... 
}
```
`*`就是通配符，代表所有标签的意思，意思是所有的标签都会被设置成这样的样式，该种方法在开发几乎不用。

# 字体样式

## 字体大小

font-size属性是用来设置字体大小的，字体大小的单位有很多，其中最常用的就是px,代表的是像素，1px代表的就是一个像素的大小。比如

```css
font-size: 20px;
```

在网页中普遍使用14px+,并且尽量用偶数大小，因为奇数可能在低版本的浏览器中出现莫名的问题。

## 字体

font-family属性是用来设置字体的，比如

```css
font-family: "楷体";
```
英文字体可以不加引号，但是如果英文字体含有空格，则需要加上引号，比如

```css
font-family: "Times New Roman";
```
可以同时指定多个字体，中间以逗号隔开，表示如果浏览器不支持第一个字体，则会尝试下一个，直到找到合适的字体。

在CSS中设置字体名称，直接写中文是可以的。但是在文件编码(GB2312、UTF-8等)不匹配时会产生乱码的错误。xp系统不支持类似微软雅黑的中文。解决办法是可以使用CSS Unicode字体表示，比如 `font-family: "\5FAE\8F6F\96C5\9ED1"` 就表示设置字体为微软雅黑。其中 `\5FAE\8F6F\96C5\9ED1` 就是Unicode字体。常见中文对应的Unicode字体如下:

| 字体名称    | 英文名称        | Unicode 编码         |
| ----------- | --------------- | -------------------- |
| 宋体        |SimSun          | \5B8B\4F53           |
| 新宋体      |NSimSun         | \65B0\5B8B\4F53      |
| 黑体        |SimHei          | \9ED1\4F53           |
| 微软雅黑    |Microsoft YaHei | \5FAE\8F6F\96C5\9ED1 |
| 楷体\_GB2312 |KaiTi_GB2312    | \6977\4F53_GB2312   |
| 隶书        |LiSu            | \96B6\4E66           |
| 幼园        |YouYuan         | \5E7C\5706           |
| 华文细黑    |STXihei         | \534E\6587\7EC6\9ED1|
| 细明体      |MingLiU         | \7EC6\660E\4F53      |
| 新细明体    |PMingLiU        | \65B0\7EC6\660E\4F53 |

## 字体粗细

font-weight属性是用来设置字体粗细的，可选属性值有normal, bold, older, lighter,除了有着四种值可选外，其值还可以是数字，范围为100-900并且必须为100的倍数。normal相当于是400,bold相当于是700,一般建议用数字。

```css
font-weight: 400;
```

## 字体风格

font-style属性是用来设置字体风格的，有两种值可选，分别是normal对于正常的字体风格，另一个是italic对应的是斜体风格。

```css
font-style: italic;
```

## 综合连写

假如我们设置字体的时候，四个都要设置，假如我要设置字体大小为20px,字体为微软雅黑，字体粗细为500,字体风格为斜体，我们会这样写:

```css
.font {
    font-size: 20px;
    font-family: "\5FAE\8F6F\96C5\9ED1";
    font-weight: 500;
    font-style: italic;
}
```
因为对于字体样式写的比较频繁，这么写的话过于的繁琐，所以CSS规定可以对其综合连写，如下

```css
.font {
    font: italic 500 20px "\5FAE\8F6F\96C5\9ED1";
}
```
其中书写顺序必须为font-style, font-weight, font-size, font-family,其中font-size和font-family不可以省略。

# 文本样式

## 颜色

color属性用以设置文本的颜色，其取值有三种方法:

- red,green,blue...等内置的颜色
- 使用十六进制，比如\#FF0000，其中十六进制的前两位代表三基色中的红色，中间两位代表绿色，后两位代表蓝色。所以\#FF0000代表的就是红色。
- 使用RGB代码，比如红色为rgb(255,0,0)。

其中第二种方法使用的最多，如果十六进制的三对每两位是相同的，则可以缩写为一个，比如\#FF0000可以缩写为\#F00。\#FFDD66可以缩写为\#FD6。

## 行高

line-height属性用以设置行高，比如对一个段落来说，其行与行之间的距离太小了，则可以通过设置行高来调整行间的距离。

```css
p{
    line-height: 40px;
}
```
## 文本对齐方式

text-align用以设置文本的水平对齐方式，可选值有三个，分别为left, right, center。

```css
text-align: center;
```

## 缩进

text-indent用以设置段落首行的缩进距离，一般设置为2em即两个字的距离：

```css
text-indent: 2em;
```

## 文本修饰

text-decoration属性用来修饰文本，有一下几个值可选:

- none：取消文本修饰或无修饰
- underline：下划线
- overline：上划线
- line-through：删除线

# 复合选择器

## 后代选择器

假如有下面这么一个例子

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>后代选择器</title>
    </head>
    <body>
        <div>
            <p>开心</p>
            <p>快乐</p>
        </div>

        <p>乐观</p>
    </body>
</html>
```

我们希望将开心和快乐变成红色，而不影响乐观，即将div标签下的p标签设置为红色，但不是所有的p标签设置为红色，所以不能这么写

```html
p {
    color: red;
}
```

也许你们可能会写一个类，然后让这两个p标签去调用，但是假设div下有很多这样的p标签的话，这样做就不现实，我这里使用的是后代选择器，如下

```html
div p {
    color: red;
}
```
即设置div标签下的p标签设置为红色。因为乐观所处的p标签不在div标签下，所以乐观不受影响。效果如下:

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/css02.png"/>
</center>

## 子代选择器

假设有这么一个案例

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>子代选择器</title>
    </head>
    <body>
        <ul>
            <li><a href="#">一级菜单</a></li>
            <li><a href="#">一级菜单</a></li>
            <li><a href="#">一级菜单</a></li>
            <li>
                <div><a href="#">二级菜单</a></div>
            </li>
        </ul>
    </body>
</html>
```

我们希望一级菜单变为红色，而二级菜单不受到影响，如果我们使用后代选择器

```html
ul li a {
    color: red;
}
```
那么li标签下的所有a标签都会受到影响，二级菜单也会受到影响，这个时候我们需要用子代选择器

```html
ul>li>a {
    color: red;
}
```

上面便是子代选择器的写法，与后代选择器不同，子代只包括"儿子",而后代则是包括后代所有的。所以使用子代选择器，受到影响的只有li标签下的a标签，二级菜单没有受到影响。效果为:

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/css03.png"/>
</center>

## 交集选择器

假设有下面这么一个案例

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>交集选择器</title>
    </head>
    <body>
        <div>啊啊</div>
        <div>啊啊</div>
        <div>啊啊</div>
        <p>一一</p>
        <p>一一</p>
        <p>一一</p>
    </body>
</html>
```
我们希望将第三个div标签中的文字设置为红色。很简单的一个方法就是这样

```html
.red {
    color: red;
}
```

然后让第三个div标签去调用，但是我们这里做出要求，当p标签去调用这个类时，它的颜色不会改变。这个时候我们就要用到交集选择器

```html
div.red {
    color: red;
}
```

上面的选择器表明，只有div标签调用这个类才会生效，其他标签调用这个类不会生效，这就是交集选择器。

## 并集选择器

假设又有下面这个案例

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>并集选择器</title>
    </head>
    <body>
        <div>123</div>
        <p>123</p>
        <h1>123</h1>

        <a href="#">123</a>
        <strong>123</strong>
    </body>
</html>
```
我们希望div,p.h1标签都设置为红色，其他的不变，我们可以这样写

```html
div {
    color: red;
}
p {
    color: red;
}
h1 {
    color: red;
}
```

使用并集选择器可以使上面的代码大大减少

```html
div, p, h1 {
    color: red;
}
```

使用逗号将标签或者类名隔开。

## 链接伪类选择器

什么是伪类选择器，伪类选择器是向某些选择器添加特殊效果，比如为链接添加特殊效果,链接伪类选择器有四个，分别为

- link：未访问过的链接状态
- visited：已访问过的链接状态
- hover：鼠标放上去时的链接状态
- active：鼠标按下时的链接状态

类选择器使用点.,而伪类选择器使用冒号:。现在我们有这个需要，当未访问链接时，链接字体为25px,无下划线，当鼠标放上去时，颜色变为红色，当按下时颜色变为橙色，当访问后，颜色为绿色。CSS样式按如下写

```css
a:link {
    font-size: 25px;
    text-decoration: none;     /*取消下划线*/
}
a:visited {
    color: green;
}
a:hover {
    color: red;
}
a:active {
    color: orange;
}
```

效果为

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/css05.gif"/>
</center>

当未按下链接时，链接为蓝色，按把鼠标放上去时，链接变为红色，当按下链接时，链接变为橙色，当访问完链接后，链接变为了绿色。需要**注意**的一点是，伪类选择器的顺序必须按照lvha的顺序来，否则会达不到想要的效果。在实际的开发中，一般只用到hover,不会写的这么复杂，即我们只需要当鼠标放上去变颜色就行，如下

```css
a {
    font-size: 25px;
    text-decoration: none;
}
a:hover {
    color: red;
}
```

# 标签显示模式

我们把标签分为三类，一类为块级(block)标签，一类为行内(inline)标签,最后一类为二者的综合，为行内块(inline-block)标签。每个块标签通常都会独自占据一整行或多整行，可以对其设置**宽度、高度、对齐**等属性，常用于网页布局和网页结构的搭建。常见的块级标签包括

```html
<h1>~<h6>、<p>、<div>、<ul>、<ol>、<li>
```

行内标签（内联标签）不占有独立的区域，仅仅靠自身的字体大小和图像尺寸来支撑结构，一般**不可以**设置**宽度、高度、对齐**等属性，常用于控制页面中文本的样式。常见的行内标签包括

```html
<a>、<strong>、<b>、<em>、<i>、<del>、<s>、<ins>、<u>、<span>
```

假设我们对行内标签设置其宽高属性

```css
a {
    width: 20px;
    height: 30px;
}
```

这样做是没有效果的。链接a的宽高属性不会改变。

在行内元素中有几个特殊的标签——`<img />`、`<input />`、`<td>`，**可以对它们设置宽高和对齐属性**，有些资料可能会称它们为行内块标签。那么标签的这种显示模式可不可以转换呢? 答案是可以，我们可以使用display属性对其进行转换，如

```css
display: inline;       /*将块级标签转换为行内标签*/
display: block;        /*将行内标签转换为块级标签*/
display: inline-block; /*将块级标签或者行内标签转换为行内块标签*/
```

我们来看一个例子，假设有下面的程序

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>标签显示模式</title>
        <style>
            a {
                text-decoration: none;  /*取消下划线*/
                background: pink;       /*设置背景颜色，用以观察该标签显示模式的转换*/
            }
        </style>
    </head>
    <body>
        <a href="#">链接</a>
        <a href="#">链接</a>
        <a href="#">链接</a>
    </body>
</html>
```

因为标签a是行内标签，所以这三个标签是显示在一行内的，如下:

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/css06.png"/>
</center>

并且无法对其设置宽高以及对齐属性。现在我们将标签`a`的显示模式改为块级模式

```css
a {
    text-decoration: none;
    background: pink;
    display: block;
}
```

效果为

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/css07.png"/>
</center>

可见链接已经变为了块级标签，一个标签占据一整行，并且我们可以对其设置宽高属性以及对其方式，如下

```css
a {
    text-decoration: none;
    background: pink;
    display: block;
    width: 400px;
    height: 100px;
    text-align: center;
}
```
效果为

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/css08.png"/>
</center>

可见链接a的宽高发生了变化，并且其内的文字水平居中对齐了。我们可以把所有的标签看做是一个盒子，有的盒子是占据一行的，并且可以设置其大小，有的盒子只能在行内，并且不能设置其大小，还有的盒子虽然在行内，但是可以设置其大小。我们就用CSS对盒子进行操作，或摆放，或修饰，或改变盒子的显示模式等等。

# 导航栏练习

现在我们要做一个导航栏，其最终效果如下

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/css09.gif"/>
</center>

我们要求导航栏的布局居中显示，并且其内的文字也要居中对齐。并且当鼠标放在网站导航上，其背景图片发生变换。

首先第一步便是显示着六个链接，如下

```html
<div class="nav">
    <a href="#">网站导航</a>
    <a href="#">网站导航</a>
    <a href="#">网站导航</a>
    <a href="#">网站导航</a>
    <a href="#">网站导航</a>
    <a href="#">网站导航</a>
</div>
```
效果如下:

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/css10.png"/>
</center>

第二步，取消下划线，并且设置字体颜色为白色，并且添加背景图片，由于链接标签a是行内标签，其大小不可以改变，为了使添加的背景图片吻合，还需要改变其显示模式为display: inline-block,然后设置其宽高属性为图片宽高的大小。如下:

```css
.nav a {
    text-decoration: none; /*取消下划线*/
    color: #FFF; /*设置字体颜色为白色*/
    display: inline-block;
    width: 120px;
    height: 50px;
    background-image: url(bg.png);
}
```

在这里，为了只影响导航栏的链接标签，这里我们用了后代选择器。效果如下:

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/css11.png"/>
</center>

这个时候我们发现导航栏没有居中对齐，并且里面的文字没有居中对齐

```css
.nav {
    text-align: center;
}
```

我们设置.nav类里的文字居中对齐，但是div里面的是链接，text-align属性还有用吗? 答案是可以，链接在块类标签里面可以当做是文字处理，这是一个知识点，记住了。效果如下：

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/css12.png"/>
</center>

我们发现导航栏居中对齐了现在我们需要将链接里面的文字水平居中对齐和垂直居中对齐，在.nav a加入如下

```css
text-align: center;
line-height: 50px;  /*当行高等于标签设置的高度时，会使文字垂直居中对齐*/
```
效果如下

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/css13.png"/>
</center>

现在最后一步就是将鼠标放上去，背景图片改变，我们使用伪类hover,如下

```css
.nav a:hover {
    background-image: url("bgc.png");
}
```

这里为了只对导航栏的链接生效，这里也用了后代选择器，效果如下:

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/css14.gif"/>
</center>

到这里我们已经完整的实现了导航栏的案例，下面贴出完整的代码。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>导航栏案例</title>
    <style>
        .nav a {
            text-decoration: none; /*取消下划线*/
            color: #FFF; /*设置字体颜色为白色*/
            display: inline-block;
            width: 120px;
            height: 50px;
            background-image: url(bg.png);
            text-align: center;
            line-height: 50px;
        }

        .nav a:hover {
            background-image: url("bgc.png");
        }

        .nav {
            text-align: center;
        }

    </style>
</head>
<body>
    <div class="nav">
        <a href="#">网站导航</a>
        <a href="#">网站导航</a>
        <a href="#">网站导航</a>
        <a href="#">网站导航</a>
        <a href="#">网站导航</a>
        <a href="#">网站导航</a>
    </div>
</body>
</html>
```

# 行高

在之前我们有提到这个属性line-height，该属性用来设置行高，并且我们用它来设置过行间距。为了明白行高指的是什么之间的距离，我们需要明白下面这个图

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/css15.png"/>
</center>

一个文本内容被四条线划分，而行高指的就是基线与基线之间的距离，当不设置行高时，行高的大小为字体的大小，所以行与行之间是紧贴着的。

在导航栏练习中，为了使文字能够垂直居中对齐，我们让line-height的大小等于盒子的大小，为了解释这一现象，我们先看一下盒子的组成，盒子的高度由上距离，下距离和文本内容的高度组成，上距离的大小为(行高-内容高度)/2,下距离的高度=盒子高度-上距离的高度-内容高度。当文本垂直居中显示时，上距离的高度等于下距离的高度，这时上距离的高度=(盒子高度-内容高度)/2,对比于上距离高度的公式，得到此时行高等于盒子的高度。所以这就解释了为什么当行高等于盒子高度时，文字会垂直水平居中，当行高增大时，上距离的高度增大，文字会向下移动。






