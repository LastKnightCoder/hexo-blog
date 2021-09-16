---
title: offsetX,pageX,clientX和screenX
categories: 
	- JavaScript
tags:
	- JavaScript
keywords: JavaScript中的各种距离
description:
    讲解JavaScript中的各种距离
date: 2020-06-03
cover: https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200603111110.png
---

在使用的 `JavaScript` 的过程中，对 `offset`，`page`，`client` 等等所表示的距离一直不是很清楚，现趁有时间系统的学习了一下。上面所述的 `offsetX`，`pageX` 等等都是事件对象 `event` 的属性，当有鼠标事件触发时，可以获得相应的事件对象，该对象中包含着鼠标的各种距离，现总结如下

- `offsetX`：离触发事件元素左边的距离
- `pageX`：离页面左边的距离
- `clientX`：离浏览器左边的距离
- `screenX`：离电脑显示器左边的距离

现在就演示一下，假设有下面的页面结构

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200603101028.png" width="80%"/>

上图已经基本可以明白 `offsetX` 和 `clientX`的大小，如果没有横向的滚动条的话，`pageX` 与 `clientX` 的大小是一样的。如果有滚动条的话，那么 `pageX` 的距离一般是大于 `clientX` 的距离的，如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200603102818.png"/>

上图应该已经阐释了 `pageX` 与 `clientX` 的不同，那么只剩最后一个 `screenX`，这个也较好理解，就是鼠标位置离电脑显示器左侧的位置，如果浏览器是最大化的话，屏幕的左侧与浏览器的左侧相同，那么 `screenX` 的大小与 `clientX` 的大小相同，如果不是最大化，那么

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200603103752.png"/>

上面介绍的是横向距离，纵向距离也是相似，`offsetY`，`pageY`，`clientY`，`screenY` 计算将左侧距离换为上侧距离。

> 这里还可以在扩展一个距离，`scrollX` 指的是横向滚动的距离，其实通过分析可以发现
> $$
> scrollX = pageX - clientX
> $$
> 如下图所示
>
> <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200603104625.png"/>