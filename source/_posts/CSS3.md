---
title: CSS3 新特性
categories: 
	- CSS
tags:
	- CSS3
keywords: CSS3 新特性
mathjax: true
description:
    讲解CSS3的新特性 几乎包括了各个方面
date: 2019-12-25
cover: https://gitee.com/lastknightcoder/blogimage/raw/master/img/bg29.jpg
---

## CSS3新特性样式

### 背景

#### background-origin

我们知道盒子的大小有三部分组成：border, padding, content，当我们设置背景图片时，图片是会以左上角对齐，但是是以border的左上角对齐还是以padding的左上角或者content的左上角对齐? border-origin正是用来设置这个的，它有三个可选值

<!--more-->

- border-box
- padding-box
- content-box

其中意思不必解释就可以明白。如果不进行设置的话，默认是padding-box，即以padding的左上角为原点。

#### background-clip

该属性是用来设置背景(背景图片、背景颜色)延伸的范围，有4个值可选

- border-box：背景延伸至边框外沿(但是在边框下层)
- padding-box：背景延伸至内边距(padding)外沿,不会绘制到边框处
- content-box：背景被裁剪至内容区(content box)外沿
- text：背景被裁剪成文字的前景色

具体可以参考网站[background-clip](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)，这里演示一下上面的效果

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS301.gif">
</center>

#### background-size

background-size用以设置背景图片大小。单张图片的背景大小可以使用以下三种方法中的一种来规定：

- 使用关键词 contain
- 使用关键词 cover
- 设定宽度和高度值

设定指定的宽度和高度值想必不用多加介绍。contain和cover会等比例的缩放图片，以使得图片能够最大的被完整包含或者最小的覆盖背景区。

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS302.gif">
</center>

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS303.gif">
</center>

如果背景区(由background-origin决定)的宽高比和图片的宽高比是一样的，那么cover和contain的结果是一样的，会完全的覆盖背景区并完整的显示。

### 边框

#### 边框圆角

使用border-radius可以设置边框为圆角的，border-radius的值就是圆角边框的半径。

<center>
<img src="https://img-blog.csdnimg.cn/20190121192025792.png">
</center>

```css
width: 100px;
height: 100px;
margin: 0 auto;
border-radius: 20px;
background-color: pink;
```
<center>
<img src="https://img-blog.csdnimg.cn/20190121192340237.png">
</center>

与padding一样，取不同个数的值，代表设置不同地方的圆角半径，如

取值个数|设置
---|---
1个，如border-radius: 20px|设置四个角的圆角半径都为20px
2个，如border-radius: 10px 20px|设置左上角和右下角这条对角线为10px,另一条对角线为20px
3个，如border-radius: 10px 20px 30px|设置左上角为10px,右上角和左下角这条对角线为20px,右下角为30px
4个，如border-radius: 5px 10px 15px 20px|从左上角开始设置，按顺时针来，即左上角为5px,右上角为10px, ...

由第一张图，我们发现半径也分为水平半径和垂直半径，这两个也可以分别设置，用`/`分开当做两组，第一个用来设置水平半径，第二个设置垂直半径，如

```css
width: 100px;
height: 120px;
margin: 0 auto;
border-radius: 50px / 60px; 
background-color: pink;
```

我们设置四个角的水平半径为50px,四个角的水平半径为60px,得到效果如下

<center>
<img src="https://img-blog.csdnimg.cn/20190121193308161.png">
</center>

如果不分开设置，则默认水平半径和垂直半径是相同的。

如果想要更加详细的了解，这里推荐一篇阮一峰的网络日志[CSS3圆角边框](http://www.ruanyifeng.com/blog/2010/12/detailed_explanation_of_css3_rounded_corners.html)。

#### 边框图片

找到一篇写的很好的博文[border-image的正确用法](https://aotu.io/notes/2016/11/02/border-image/index.html)，所以这里就不自己写了。

### 阴影

#### 盒子阴影

为了知道什么是盒子阴影，观看如下效果
<center>
<img src="https://img-blog.csdnimg.cn/20190123143605852.gif">
</center>

这里选取了小米的做法，由于设置阴影的颜色比较淡，所以可能比较难看出来。

用以设置盒子阴影的属性是box-shadow,它的值比较多，需要设置的值如下，

- 水平阴影 
- 垂直阴影 
- 模糊距离(虚实) 
- 阴影尺寸(影子大小)  
- 阴影颜色  
- 内/外阴影(这个我也不懂，不做介绍)

其中水平阴影h-shadow和垂直阴影v-shadow是必须设置的。

为了观看设置这些值的效果，我们首先创建一个盒子
```css
width: 200px;
height: 200px;
border: 1px solid #CCC;
margin: 100px auto;
```
<center>
<img src="https://img-blog.csdnimg.cn/20190123152039866.png">
</center>

为盒子添加阴影

```css
box-shadow: 1px 1px 1px 1px red;
```

水平阴影大小的影响(正值，阴影向右移动，负值，阴影向左移动)
<center>
<img src="https://img-blog.csdnimg.cn/20190123153228640.gif">
</center>

垂直阴影大小的影响(正值，阴影向下移动，负值，阴影向上移动)
<center>
<img src="https://img-blog.csdnimg.cn/20190123153439339.gif">
</center>

模糊距离大小的影响
<center>
<img src="https://img-blog.csdnimg.cn/20190123153534212.gif">
</center>

阴影尺寸大小的影响
<center>
<img src="https://img-blog.csdnimg.cn/20190123153544203.gif">
</center>

#### 文本阴影

使用text-shadow来设置文本阴影。使用与box-shadow差不多，具体可以参照[text-shadow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-shadow)。


## CSS3选择器

CSS3新增了许多的选择器，为我们选择元素提供了更加灵活的选择。

### 属性选择器

- [attr]：选择包含attr属性的标签
- [attr=value]：选择attr属性值为value的标签
- [attr^=value]：选择attr属性值以value开头的标签
- [attr*=value]：选择attr属性值包含value的标签
- [attr$=value]：选择attr属性值以value的标签

如
```css
div[class] {
    /* 会选择包含class属性的div标签 */
}
div[class="active"] {
    /* 会选择class属性值为active的div标签 */
}
div[class^="header"] {
    /* 会选择class属性以header开头的div标签 */
}

... ...
```

### 结构伪类选择器

- E:first-child
- E:last-child
- E:nth-child(n)
- E:nth-last-child(n)
- E:first-of-type
- E:last-of-type
- E:nth-of-type(n)
- E:nth-last-of-type(n)

上面的选择器是比较常见的结构伪类选择器，下面具体讲解其表达的意思。

E:first-child指的是，选择E，这个E满足的条件是：它是其父元素第一个子元素。听起来有点绕，来看一个例子

```html
<div>
    <span></span>
    <h1></h1>
    <p></p>
</div>
<div>
    <i></i>
    <span></span>
    <h1></h1>
    <p></p>
</div>
```

```css
span:first-child {
    /* 它会选择span，条件是这个span必须是它父元素的第一个子元素 */
}
```

由于第一个div里面的span是其父元素(第一个div)的第一个子元素，所以第一个div里面的span会被选中。而第二个div里面的span不是其父元素(第二个div)的第一个子元素，所以这个span不会被选中。

E:last-child的与E:first-child相似，不过第一个改为最后一个。

E:nth-child(n)指的是选择E，E需要满足条件，是其父元素的第n个元素(n是从1开始的)，E:first-child就相当于是E:nth-child(1)。E:nth-child(n)中这个"n"除了可以是具体的数字以外，还可以是odd和even，表示选择所有的E，这些E是其父元素的第奇数或第偶数个。除此之外"n"还可以是表达式，如2n+1, 3n(n从1开始)。

而E:nth-last-child(n)则是倒着数的，用法同E:nth-child(n)相似，这里不多介绍。

E:first-of-type与E:first-child不同，其意思是选择器父元素下的第一个E元素。还是以上面两个div为例

```html
<div>
    <span></span>
    <h1></h1>
    <p></p>
</div>
<div>
    <i></i>
    <span></span>
    <h1></h1>
    <p></p>
</div>
```

```css
span:first-of-type {
    /* 选择span父元素下的第一个span元素 */
}
```

这个时候两个span都可以被选择到，first-of-type相当于只是将E的父元素所包含的E全部抽离出来，然后进行选择。现在我们将div包含的span全部抽离出来，相当于

```html
<div>
    <span></span>
</div>
<div>
    <span></span>
</div>
```

然后选择第一个span，所以这两个span都可以被选择到。其余的first-last-type, nth-of-type(n), nth-last-of-type同上面介绍的last-child, ...相似，其不同之处在fitst-child与first-of-type处已详细阐述。

## CSS3颜色渐变

颜色渐变是指在两个颜色之间平稳的过渡。以往我们如果希望有颜色渐变的效果，会在绘图工具(如PS)设计出希望的效果，然后作为图片来实现这种效果。现在通过浏览器可以渲染而成，这样可以减少下载的时间和带宽的使用，以及在放大时看起来效果更好，因为这是浏览器自动生成的。

### 线性渐变

线性渐变指的是颜色在一条线上平稳的变化，为了实现线性渐变，我们要规定线的方向，起点颜色和终止颜色。它的语法为

```css
background-image: linear-gradient(direction, color-stop1, color-stop2, ...);
```

如

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <style>
        .box {
            width:100%;
            height:100px;
            background-image: linear-gradient(to right, red, green);
        }
    </style>

</head>
<body>
    <div class="box">

    </div>
</body>
</html>
```

效果为

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS04.png">
</center>

其中

```css
linear-gradient(to right, red, green);
```

其中第一个参数`to right`是用来设置方向的，设置方向的值有

- to right：从左往右
- to left：从右往左
- to top：从下往上
- to bottom：从上往下，默认值
- to right bottom：从左上角往右下角
- ... ...

除了可以设置这些值之外，还可以设置角度，如

```css
linear-gradient(0deg, red, green);
```

其中角度所代表的的方向如下所示

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS05.jpg" width="70%" title="图片来自于菜鸟教程">
</center>


即`0deg`代表的方向是从下往上。

除此之外，除了设置起点颜色和终点颜色之外，还可以在之间设置多个颜色节点，如下面设置了一个彩虹渐变色

```css
background-image: linear-gradient(to right, red, orange, yellow, green, blue, indigo, violet);
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS06.png">
</center>

除此之外，还可以在颜色后面加上数值或者百分比，如

```css
background-image: linear-gradient(to right, red, blue 10%, violet);
```

`blue 10%`表示`blue`颜色节点在该线性方向`10%`的位置，所以从`0%-10%`是红色到蓝色的渐变，`10%-100%`是蓝色到紫色的渐变。

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS07.png">
</center>

接下来介绍渐变的最后一个特性：渐变重复。看下面一个例子

```css
/* 设置为23%而没有设置为能被100%整除的数 就是想看一下这种情况是怎么处理的 */
repeating-linear-gradient(to right, red,yellow 23%);
```

上面的意思是从`0%-23%`实现红色到黄色的渐变，然后重复直至`100%`。

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS08.png">
</center>

### 径向渐变

径向渐变是指以某点为圆心，向外进行颜色渐变。所以为了实现径向渐变，我们要规定圆心的位置和起点颜色和终点颜色。径向渐变的语法为

```css
background-image: radial-gradient(shape size at position, start-color, ..., last-color);
```

其中shape为渐变的形状，有两种取值

- circle(圆形)
- ellipse(椭圆, 默认值)

size是指`100%`所代表的的长度，有四种取值

- closest-side(离最近的边的距离)
- farthest-side(离最远的边的距离)
- cloest-corner(离最近的角的距离)
- farthest-corber(离最远的角的距离，默认值)

position是指圆心的位置，默认为`center`，即中心位置，也可以通过`at 100px 100px`形式进行设置，左上角的坐标的为(0px, 0px)。

```css
.box {
    width:400px;
    height:200px;
    /* 设置size大小为离最近的边的距离 */
    background-image: repeating-radial-gradient(circle closest-side at 200px 100px, red, red 10%,green 12.5%, green 25%)
}
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS09.png">
</center>

可见离最近的边有4个完整的重复。

```css
/* 设置size大小为离最远的边的距离 */
background-image: repeating-radial-gradient(circle farthest-side at 200px 100px, red, red 10%,green 12.5%, green 25%)
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS10.png">
</center>

可见离最远的边有4个完整的重复。

```css
/*为了使最远的角和最近的角的距离不同 position不能设置在中心 */
/* 设置size大小为离最近的角的距离 */
background-image: repeating-radial-gradient(circle closest-corner at 100px 100px, red, red 10%,green 12.5%, green 25%)
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS11.png">
</center>

可见离最近的角有4个完整的重复。

```css
/* 设置size大小为离最远的角的距离 */
background-image: repeating-radial-gradient(circle farthest-corner at 100px 100px, red, red 10%,green 12.5%, green 25%)
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS12.png">
</center>

可见离最远的角有4个完整的重复。

## CSS3 2D变换

CSS3 2D变换包括对元素进行移动、缩放、转动、拉长或拉伸。

- translate()：对元素进行进行移动
  - translate(100px)：对元素向x正方向移动100px(负值向负方向移动)
  - translate(100px, 100px)：对元素向x, y正方向方向移动100px
- scale()：对元素进行缩放
  - scale(n)：对元素进行缩放，传入的参数大于1，进行放大，小于1，进行缩小
  - scale(x, y)：第一个参数对宽度进行缩放，第二个值对高度进行缩放
- rotate()：围绕中心旋转，正值顺时针，负值逆时针
  - transform-origin：可以改变旋转的中心，如

    ```css
    /* 围绕左上角进行旋转 */
    transform-origin: left top;
    ```
    ```css
    /* 围绕中心旋转 为默认值 */
    transform-origin: 50% 50%;
    ```

- skew()：对元素进行倾斜
  - skew(angle)：向x轴负方向倾斜angle(负值沿正方向)
  - skew(anglex, angley)：第一个参数对x方向，第二个参数对y方向

除了可以使用上述属性进行设置，还可以使用translateX(), translateY()等进行设置。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <style>
        div {
            width: 100px;
            height: 100px;
        }

        /* 为每个div设置颜色 */
        .box1 {
            background-color: red;
        }
        .box2 {
            background-color: aqua;
        }
        .box3 {
            background-color: chocolate;
        }
        .box4 {
            background-color: darkcyan;
        }

        /* 为每个盒子设置不同的2D变换效果 */
        .box1:hover {
            transform: translateX(100px);
        }
        .box2:hover {
            transform: scale(0.5);
        }
        .box3:hover {
            transform: rotate(-30deg);
        }
        .box4:hover {
            transform: skew(30deg);
        }
    </style>

</head>
<body>
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3"></div>
    <div class="box4"></div>  
</body>
</html>
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS313.gif">
</center>

## CSS3 3D

3D变换的操作同2D相同，只是多了一个对Z轴的操作，如translateZ()，而rotate也分为rotateX(), rotateY(), rotateZ()，分别表示绕着X轴，Y轴，Z轴旋转。2D变换的rotate()其实就相当于rotateZ()。

## CSS3动画

### transition

首先我们来看一个例子

```css
.box {
    height: 100px;
    width: 100px;
    background-color: black;
}
.box:hover {
    transform: translateX(600px);
}
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS314.gif">
</center>

当我们把鼠标放在盒子上时，盒子向右移动了600px，但是这个过程是在瞬间完成的，十分的突兀，我们希望是缓慢由一个转态转变到另一个状态的，这种效果又叫做过渡，这时就需要用到transition属性了。

为某个元素添加过渡效果，必须规定两项内容：

- transition-property：指定要添加效果的CSS属性

例如上面就要为transform添加过渡效果，所以就可以写为

```css
/* 值可以为all 表示为所有的属性添加过渡效果 默认值为all*/
transition-property: transform;
```

- transition-duration：添加过渡的总时间

```css
/* 默认值为0s */
transition-duration: 1s; /* 单位可以为s */
transition-duration: 100ms; /* 单位也可以为ms */
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS315.gif">
</center>

除了上面必须设置的两个属性，还可以设置下面的属性

- transition-timing-function：时间函数，设置过渡的变化速度
  - ease：开始和结束慢，中间快，默认值。
  - linear：匀速。
  - ease-in：开始慢。
  - ease-out：结束慢。
  - ease-in-out：和ease类似，但比ease幅度大。

除了可以设置以上关键字，还可以设置steps()函数，语法如下：

```css
steps(<integer>[,start | end]?)
```

第一个参数传入一个整数值，steps步进函数将过渡时间划分成大小相等的时间时隔来运行，这个整数值就是分成的份数。第二个值是可选的，默认值为end。如果是start，则不保留开始值，如果是end，则保留开始值。

这里给出一个利用steps()函数做动画效果的[例子](https://cssanimation.rocks/twitter-fave/)。

- transition-delay：延时时间，默认值为0s

### animation

与transition一样，animation也有很多的属性

- animation-name：动画名称
- animation-duration：动画持续时间
- animation-timing-function：动画时间函数
- animation-delay：动画延迟时间
- animation-iteration-count：动画执行次数，可以设置为一个整数，也可以设置为infinite，意思是无限循环
- animation-direction：动画执行方向
- animation-paly-state：动画播放状态
- animation-fill-mode：动画填充模式

过渡是指在两个状态之间，而动画则是指在多个状态之间变化，这些状态我们称之为关键帧，因此动画又称之为关键帧动画。使用动画首先要创建关键帧，然后使用animation-name去调用该动画，如

```html
 <!DOCTYPE html>
 <html lang="en">
 <head>
     <meta charset="UTF-8">
     <title>Title</title>
 
     <style>
         .box {
             width: 100px;
             height: 100px;
             background-color: aqua;
         }
         
         .box:hover {
            animation-name: move;
            animation-duration: 1s;
         }

        /* 创建一个关键帧动画 动画名为move */
        /* 调用时使用animation-name: move调用 */
         @keyframes move {
             /* 第一个状态 0% 时的状态 可以使用from代替 */
             /* 这里的百分比是指 总时间 * 百分比 得到的某个时刻的状态*/
             0% {
                 transform: translateX(0px);
             }

             50% {
                 transform: translateX(100px);
             }

             /* 100% 可以使用 to 代替 */
             100% {
                 transform: translateY(100px);
             }
         }
     </style>
 
 </head>
 <body>
    <div class="box">
         
    </div>
 </body>
 </html>
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS316.gif">
</center>


我们还可以设置动画的执行次数，修改上面的样式为

```css
.box {
    width: 100px;
    height: 100px;
    background-color: aqua;
    animation-name: move;
    animation-duration: 1s;
    /* 播放次数默认值为 1 */
    animation-iteration-count: infinite;
}
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS317.gif">
</center>


我们发现动画执行完毕后，盒子突然就回到了原位置，这个过程有点突兀，我们希望这个过程也是慢慢过渡的，那么我们可以设置animation-direction，animation-direction有四种取值

- normal：默认值，正常播放
- reverse：反向播放
- alternate：如果动画次数在两次或两次以上，那么第偶数次为反向播放，就可以达到回到原位置时也有过渡的效果。若动画只播放一次，则和正向播放一样。
- alternate-reverse：若动画只播放一次，则和反向播放一样。若播放两次以上，偶数次效果为正向播放

```css
.box:hover {
   animation-name: move;
   animation-duration: 1s;
   animation-iteration-count: 2;
   animation-direction: alternate;
}
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS318.gif">
</center>

animation-play-state用来设置动画的播放状态，有两种取值

- running：默认值，动画运行
- paused：动画暂停

将样式修改为

```css
.box {
    width: 100px;
    height: 100px;
    background-color: aqua;
    animation-name: move;
    animation-duration: 1s;
    animation-iteration-count: infinite;
    animation-direction: alternate;
}
         
.box:hover {
    /* 当鼠标放上去时 动画暂停 */
   animation-play-state: paused;
}
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS319.gif">
</center>

animation-fill-mode用来定义动画开始帧之前和结束帧之后的动作，有以下四种取值

- none：默认值。动画结束后，元素移动到初始状态(不一定0%的状态，是元素自身的属性值)
- forwards：元素停在动画结束时的位置(不一定是100%的位置，有可能反向运动)
- backwards：在animation-delay的时间内，元素立刻移动到动画开始时(不一定是0%，有可能反向运动)的位置。若元素无animation-delay时，与none的效果相同
- both：同时具有forwards和backwards

修改样式如下

```css
.box {
   width: 100px;
   height: 100px;
   background-color: aqua;
}
         
.box:hover {
   animation-name: move;
   animation-duration: 1s;
   /* 动画结束后，停留在动画结束的位置 */
   animation-fill-mode: forwards;
}
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS320.gif">
</center>

## CSS3 flex布局

当一个父元素被设置为display:flex时，它就是弹性布局，子元素的float、clear和vertical-align属性将失效。此时我们把父元素称之为container(容器)，把子元素称之为item(项目)。当父元素被设置为弹性布局后，对子元素有什么影响? 先来感受一下

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <style>
        .container {
            display: flex;
            width: 500px;
            height: 400px;
            background-color: crimson;
        }

        .item {
            width: 100px;
            height: 100px;
            background-color: aqua;
            position: relative;
        }
        
        /* 这个是为了演示方便 样式不重要 不必细看 */
        .item div{
           width: 50%;
           height: 50%;
           position: absolute;
           /* 下面三行语句是为了设置居中对齐 */
           top: 50%;
           left: 50%;
           transform: translate(-50%, -50%);
           background-color: brown;
           /* 圆角边框 */
           border-radius: 50%;
           /* 下面两行设置文字居中对齐 */
           text-align: center;
           line-height: 50px;
           /* 设置字体样式 */
           font-size: 30px;
           font-weight: 700;
           color: white;
        }
    </style>

</head>
<body>
   <div class="container">
        <div class="item"><div>1</div></div>
        <div class="item"><div>2</div></div>
        <div class="item"><div>3</div></div>
   </div>
</body>
</html>
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS330.png">
</center>


当父元素设置为flex布局后，其子元素item会在父元素container的主轴上排列。

这里又牵涉到新的概念，主轴(main axis)，flex容器的主轴默认为X轴，即从左向右，既然有主轴，那么就会有侧轴(cross axis)，默认的侧轴为Y轴，即从上往下。

### container上的属性

直观的感受了一下flex的效果，现在我们来看看flex要设置哪些属性，首先设置在container的flex属性有

- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

下面来一一介绍。

flex-direction是用来设置主轴的，它有以下四个值可选

- row：默认值，主轴为水平方向，起点在左端。
- row-reverse：主轴为水平方向，起点在右端。
- column：主轴为垂直方向，起点在上沿。
- column-reverse：主轴为垂直方向，起点在下沿。

现在我们分别修改container里面的样式

```css
flex-direction: row-reverse;
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS331.png">
</center>

```css
flex-direction: column;
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS323.png">
</center>


```css
flex-direction: column-reverse;
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS324.png">
</center>

为了演示flex-wrap，我们添加9个item(此时flex-direction为默认值row，除非特别声明，否则我们在演示其一个flex属性时，会将其他flex属性设置为默认值)

```html
<div class="container">
     <div class="item"><div>1</div></div>
     <div class="item"><div>2</div></div>
     <div class="item"><div>3</div></div>
     <div class="item"><div>4</div></div>
     <div class="item"><div>5</div></div>
     <div class="item"><div>6</div></div>
     <div class="item"><div>7</div></div>
     <div class="item"><div>8</div></div>
     <div class="item"><div>9</div></div>
</div>
```

效果为

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS332.png">
</center>

我们发现9个item排成了一排，没有换行。为了使得item能够换行，我们需要设置flex-wrap，它有三个值可选

- nowrap：默认值，不换行
- wrap：换行，第一行在上方
- wrap-reverse：换行，第一行在下方

```css
flex-wrap: wrap;
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS333.png">
</center>

```css
flex-wrap: wrap-reverse;
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS334.png">
</center>

flex-flow是flex-direction和flex-wrap两个属性的简写，默认值为row wrap

```css
flex-flow: <flex-direction> || <flex-wrap>;
```

justify-content是用来设置item在container在主轴上的对齐方式的，具体对齐方式与轴的方向有关,下面假设主轴为从左到右(row)。有下面几种值可选

- flex-start：左对齐
- flex-end：右对齐
- center：居中对齐
- space-between：两端对齐，项目之间间隔相等
- space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

这里演示一下space-between和space-around

```css
justify-content: space-between;
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS328.png">
</center>

```css
justify-content: space-around;
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS329.png">
</center>

align-items定义了在侧轴上的对齐方式(以从上往下的侧轴为例)

- first-start：往上对齐
- first-end：往下对齐
- center：居中对齐
- baseline：item的第一行文字的基线对齐。
- stretch：默认值，如果没有设置height或者height设置为auto，将占满整个容器的高度

这里演示一下stretch，不设置height

```css
.container {
    display: flex;
    width: 500px;
    height: 400px;
    background-color: crimson;
    align-items: stretch;
}
.item {
    width: 100px;
    /* height: 100px; */
    background-color: aqua;
    position: relative;
}

.item div {
    /* 修改line-height为200px使文字居中 */
}
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS335.png">
</center>

align-content，当我们设置flex-wrap为wrap或者wrap-reverse时，item会占据多行，这个属性是用来设置占据多行item在侧轴上的对齐方式，如果没有多行，这个属性无效。有下列值可选(以侧轴为从上往下为例)

- first-start：往上对齐
- first-end：往下对齐
- center：居中对齐
- stretch：默认值，轴线占据整个侧轴
- space-between：两端对齐，轴线之间平均分布
- space-around

### item上的属性

下面6个属性是设置在item上的

- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- self-align

order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0，现在设置第2个item的order为1

```css
.item:nth-child(2){
    order: 1;
}
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS336.png">
</center>

如果存在剩余空间，那么flex-grow可以用来设置item占据剩余空间的份数(这会导致item增大)，默认值为0，意味着即使有剩余空间，也不增大，现在设置各自的flex-grow

```css
.item:first-child {
    /* 第一个item占 1 / (1 + 2 + 1) = 1/4 的剩余间 */
    flex-grow: 1;
}
.item:nth-child(2) {
    /* 第二个item占 2/4 的剩余空间*/
    flex-grow: 2;
}
.item:last-child {
    /* 第三个item占 1/4 的剩余空间 */
    flex-grow: 1;
}
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS337.png">
</center>

flex-shrink与flex-grow相反，当空间不足时，item缩小的以使得所有item被包含在container中。默认值是1，即每个item会等比例缩小

```css
.item:first-child {
    flex-shrink: 2;
}
.item:nth-child(2) {
    flex-shrink: 2;
}
.item:last-child {
    flex-shrink: 2;
}
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS338.png">
</center>

可见第一个、第二个和最后一个缩小的更多。

flex-basis的含义是item在被放进container之前的大小。也就是item理想或假设的大小。默认值为auto，如果不设置这个值，并且主轴是row的话，那么flex-basis就是width的大小，如果主轴是column的话，那么flex-basis就是height的大小，如果width或height也没有设置的话，flex-basis是content的大小。item的宽度是最终的flex-basis，最佳的方法是只使用flex-basis而不是width或height属性。

但是flex-basis不能保证其大小! 一旦将items放入flex容器中，flex-basis的值就无法保证了。这是因为有flex-grow和flex-shrink，可能会被放大或者缩小item的大小。

除此之外，flex-basis还受到min-width, max-width, min-height, max-height的约束。具体见[The Difference Between Width and Flex Basis](https://gedd.ski/post/the-difference-between-width-and-flex-basis/)这篇文章，有着十分详细的阐述，绝对让你物超所值。

slef-align属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

```css
.container {
    ... ...
    align-items: center;
    flex-wrap: wrap;
}
.item:first-child {
    align-self: flex-start;
}
```

<center>
<img src="https://raw.githubusercontent.com/LastKnightCoder/img/master/CSS339.png">
</center>