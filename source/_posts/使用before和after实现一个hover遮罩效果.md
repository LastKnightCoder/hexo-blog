---
title: 使用before和after实现一个hover遮罩效果
categories: 
	- CSS
tags:
	- CSS
keywords: CSS特效
description:
    使用before和after实现一个hover遮罩效果
date: 2020-04-05
cover: https://gitee.com/lastknightcoder/blogimage/raw/master/img/bg80.jpg
---

今天开始学习`CSS`效果，记录一下，成品如下

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/202004030004.gif"/>
</center>

实现的思路很简单，使用到了`::before`和`::after`两个伪元素，默认两个伪元素的`width`为`0`，`height`与父元素的高度相同，当鼠标放上去时，伪元素的`width`变为`100%`，注意到两个伪元素变化的方向不一样，因为`::before`被设置为了

```css
top: 0;
left: 0;
```

而`::after`被设置为

```css
bottom: 0;
right: 0;
```

`talk is cheap, show me the code`，直接上代码吧

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        html,
        body {
            width: 100%;
            height: 100%;
            background-color: #fff;
        }

        a {
            font-size: 50px;
            display: block;
            width: 300px;
            height: 100px;
            line-height: 100px;
            margin: auto;
            text-decoration: none;
            text-align: center;
            text-transform: uppercase;
            color: #666;
            border: 1px solid black;
            position: absolute;
            top: 0;
            left: 0;
            bottom: 0;
            right: 0;
            font-family: Arial, Helvetica, sans-serif;
            transition: all 0.5s;
            z-index: 1;
        }

        a::before,
        a::after {
            content: "";
            display: block;
            width: 0;
            height: 100px;
            position: absolute;
            background: #333;
            transition: all 0.5s;
            z-index: -1;
        }

        a::before {
            top: 0;
            left: 0;
        }

        a::after {
            bottom: 0;
            right: 0;
        }
		
        a:hover::before,
        a:hover::after {
            width: 100%;
        }
    </style>
</head>
<body>
    <a href="#">Move</a>
</body>
</html>
```

上面的代码不难理解，就一个地方需要注意的是，在`a`标签中设置了

```css
margin: auto;

position: absolute;
top: 0;
left: 0;
bottom: 0;
right: 0;
```

这样的写法，这样做的目的是使得`a`标签在`body`中居中显示，因为设置了定位的属性

```css
top: 0;
left: 0;
bottom: 0;
right: 0;
```

四个都为0，要达到这样的效果，设置`margin`为`auto`会为`a`元素四周充满外边距，如下

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200403002007.png"/>
</center>

这样就可以到居中的效果。