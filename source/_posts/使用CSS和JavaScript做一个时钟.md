---
title: 使用CSS和JavaScript做一个时钟
categories: 
	- CSS
tags:
	- CSS
	- JavaScript
keywords: CSS特效
date: 2020-07-08 23:31
---

在 `Youtube` 看到一个使用 `CSS` 和 `JavaScript` 实现的时钟效果，觉得是个比较好的练手项目，就跟着做了一个，在深入研究代码的过程中的确学习到了很多的东西，成品效果如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/clock202007072137.gif" width="70%"/>

首先我们搭建基本的页面结构，新建 `clock.html` 如下

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="clock.css">
</head>
<body>
    <div id="clock">
        <div id="hour"></div>
        <div id="minute"></div>
        <div id="second"></div>
    </div>
    <div id="digits"></div>
    <script src="clock.js"></script>
</body>
</html>
```

结构很简单，分为上下两部分，上面的部分为时钟，下面的部分为时钟的数字显示，所以我们先在 `clock.css` 写出这样的样式

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Consolas;
}

body {
    min-height: 100vh;
    background-color: #060415;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}
```

我们设置了 `body` 为一个 `flex` 盒子，并设置了水平和垂直方向都居中，因为是分为上下两部分，所以我们设置了 `flex-direction` 为 `column`。这时页面除了 `body` 的背景颜色，什么效果都没有，接下我们为 `clock` 写样式

```css
#clock {
    width: 350px;
    height: 350px;
    border: 8px solid #FFFFFF;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
}
```

同理，我们把 `clock` 也设置为了 `flex` 盒子，并使得它内部的子元素能够居中对齐，目前效果如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200708231804.png" width="70%"/>

接着我们在时钟的中间添加一个点

```css
#clock::before {
    position: absolute;
    content: "";
    width: 15px;
    height: 15px;
    background-color: #2196f3;
    z-index: 100;
    border-radius: 50%;
}
```

上面我们设置 `::before` 伪元素为绝对定位 `position: absolute;`，这是为什么呢? 当设置为绝对定位之后，**它脱离了文档流**，此时它不会受到 `flex` 盒子的影响，并且此时并没有设置 `left, top` 等的值，这时的**默认位置是它没有脱离文档流时的位置(`static position`)**。那么它没有脱离文档流时的位置在哪里，因为它在 `clock` 中，并且 `clock` 中的子元素是居中的，所以它没有脱离文档流时它是居中的，所以这时它是居中的

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200708225113.png" width="70%"/>

但是还是没有理解到为什么要设置为绝对定位，我们知道这个点，还有时针，分针和秒针全部都是在中间重合的，如果不设置绝对定位脱离文档流，这些元素是绝对不会重合的，像是这样

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200708225629.png" width="70%"/>

但是当它们设置 `position: absolute`，并且没有设置 `top, left` 等值时，它们位置会在 `static position`，在这个例子中即是居中对齐的位置。接着我们为三个针编写样式

```css
#hour, #minute, #second {
    position: absolute;
    display: flex;
    justify-content: center;
}
#hour {
    width: 200px;
    height: 200px;
}
#hour::before {
    content: "";
    width: 8px;
    height: 50%;
    background-color: #FFFFFF;
}
#minute {
    width: 230px;
    height: 230px;
}
#minute::before {
    content: "";
    width: 4px;
    height: 50%;
    background-color: #FFFFFF;
}
#second {
    width: 250px;
    height: 250px;
}
#second::before {
    content: "";
    width: 2px;
    height: 50%;
    background-color: #2196f3;
}
```

上面的代码虽长，但很多都是重复的，观察代码，我们把 `hour, minute, second` 也全部设置为了绝对定位，就是为了使得它们能够在中间重合。另外我们设置了 `hour, minute, second` 为 `flex` 盒子，并设置了水平居中，此时的时钟如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200708230715.png" width="70%"/>

接下来我们就要让它动起来，并且设置下面的时钟显示，我们使用 `JavaScript` 实现这个功能

```javascript
const hour = document.querySelector("#hour");
const minute = document.querySelector("#minute");
const second = document.querySelector("#second");

setInterval(() => {
    let day = new Date();
    let hours = day.getHours();
    let minutes = day.getMinutes();
    let seconds = day.getSeconds();

    let hourRotateDeg = hours * 30;
    let minuteRotateDeg = minutes * 6;
    let secondRotateDeg = seconds * 6;

    hour.style.transform = `rotateZ(${hourRotateDeg + minuteRotateDeg/12}deg)`;
    minute.style.transform = `rotateZ(${minuteRotateDeg}deg)`;
    second.style.transform = `rotateZ(${secondRotateDeg}deg)`;

    hours = hours < 10 ? "0" + hours : hours;
    minutes = minutes < 10 ? "0" + minutes : minutes;
    seconds = seconds < 10 ? "0" + seconds : seconds;

    let ampm = hours > 12 ? "PM" : "AM";

    let digits = document.querySelector("#digits");
    digits.innerHTML = `${hours}:${minutes}:${seconds} ${ampm}`;
}, 1000);
```

`JavaScript` 的代码还是比较简单的，设置了一个每隔 `1s` 执行一次的定时器，每次我们获得当前的时间，计算出时针、分针以及秒针旋转的角度，设置样式即可。在这其中有一个小的知识点，就是当小时、分钟、秒小于 `10` 时，我们要在前面补 `0`。

最后我们通过 `innerHTML` 设置了要显示的时钟点数，以及相关 `CSS` 如下

```css
#digits {
    color: #FFFFFF;
    font-size: 4em;
}
```

最后成品的效果如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/202007082313.gif" width="70%"/>

在最后我总结一下我在这个小小的项目获得的知识，如下

- `position: relative` 不会脱离文档流
- `position: absolute`, `float` 会脱离文档流
- `position: absolute` 未设置 `left`, `top` 等属性时，默认是 `static position` 而不是 `0`
- `display: flex` 对处于文档流(`normal flow`)的元素产生影响，对脱离文档流的元素不产生影响

