---
title: HTML基础
categories: 
	- HTML
tags:
	- HTML
	- 基础知识
keywords: HTML 基础知识
mathjax: true
description:
    讲解HTML基础知识，包含文本，图片，链接，表格，表单标签等知识
date: 2019-02-08
cover: https://gitee.com/lastknightcoder/blogimage/raw/master/img/bg15.jpg
---

## 需要的开发工具

由于目前的市场上Google的占有率最高，所以我们用Chorme作为开发用的浏览器，而写代码的工具有很多，我一般用WebStorm。

## 第一个HTML页面

一般新入手一个语言，都是先敲一个代码，然后在解释这个代码，按照惯例都是显示`Hello World`,所以这里就在网页中显示`Hello World`。

```html
<html>
    <head>
        <title>第一个页面</title>
    </head>

    <body>
        Hello World
    </body>
</html>
```

以上的程序可以写在任何文本编辑器中，然后将后缀名改为`.html`，用浏览器打开即可。

一般写html的格式应该如下

```html
<html>
    <head>
        
    </head>

    <body>
        
    </body>
</html>
```

其中`<html>,<head>`等等称为标签，在html中，内容都是由标签来呈现的，而标签分为双标签和单标签，一般我们遇到的都是双标签，比如上面的`<html></html>`是双标签，`<html>`是开始标签，`</html>`是结束标签。单标签比如`<br />`是换行标签，它的作用是换行。

其中`<body></body>`标签里写的东西才是网页呈现的内容，比如上面的`Hello World`就是写在`body`标签里面的,而`<head></head>`标签里写的东西都不会呈现在网页中，一般写的都是网页的信息，比如所用的编码，网页的标题，网页的版权等等。

我们一般会在第一行加上`<!DOCTYPE html>`,这个是用来告诉浏览器我们使用的浏览器版本，这么写的话就代表我们使用的是html5的版本。

## 排版标签

### 标题

在`html`中共有6种标题，分别是

```html
<h1></h1>
<h2></h2>
... ...
<h6></h6>
```

其中h1为最大的标题，称为一级标题，`h6`称为六级标题，上面的标题标签都是双标签。

我们在网页中看看标题的效果:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>网页标题</title>
    </head>

    <body>
        <h1>一级标题</h1>
        <h2>二级标题</h2>
        <h3>三级标题</h3>
        <h4>四级标题</h4>
        <h5>五级标题</h5>
        <h6>六级标题</h6>
    </body>
</html>
```

展示显示效果：

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html01.png"/>
</center>

</center>

### 段落

段落标签是用`<p></p>`标签来呈现的，在`<p>`标签内部的内容视为一个段落，该标签会根据浏览器窗口的大小自动换行。

```html
<!DOCTYPE html>
<html>
    <head>
        <title>网页标题</title>
    </head>

    <body>
        <p>水陆草木之花，可爱者甚蕃。晋陶渊明独爱菊。自李唐来，世人甚爱牡丹。
        予独爱莲之出淤泥而不染，濯清涟而不妖，中通外直，不蔓不枝，香远益清，
        亭亭净植，可远观而不可亵玩焉。(甚爱 一作：盛爱)</p>
        <p>予谓菊，花之隐逸者也；牡丹，花之富贵者也；莲，花之君子者也。
        噫！菊之爱，陶后鲜有闻。莲之爱，同予者何人?牡丹之爱，宜乎众矣!</p>
    </body>
</html>
```

展示显示效果：

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html02.png"/>
</center>

在`html`代码中进行换行操作时，在网页显示时不会进行换行操作，`html`中所有的功能都是由标签实现的，所以换行也需要换行标签实现。

我们可以见到，段落之间会自动换行，并且段落之间还隔有一个空行。

### 水平线

水平线标签`<hr />`是一个单标签，它的作用是显示一条水平线。

### 换行

换行标签为`<br />`,也是一个单标签，它的作用是换行，换行之后，不会像段落一样之间还有一个空行，而是两行会紧贴着。

### 两个无语义化的标签

`<div></div>`和`<span></span>`,这两个标签没有实际的功能，在后面主要是配合`CSS`进行布局，所以这里就不要管它好了。

`<div>`标签会实现自动换行，所以一行只能有一个`<div>`标签，`<span>`不会自动换行，所以一行可以有多个`<span>`标签。

我们来看一个简单的例子:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>网页标题</title>
    </head>
    <body>
        <div>会自动换行</div>
        <span>不会自动换行</span><span>可以有多个</span>
    </body>
</html>
```

展示显示效果：

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html03.png"/>
</center>

## 文本格式化

### 字体加粗

加粗标签有两个，分别为`<b></b>`和`<strong></strong>`,二者都能进行加粗，不过`<strong>`标签比较有语义，所以用的比较多。

### 斜体

斜体标签也有两个，分别为`<i></i>`和`<em></em>`,同样的，`<em>`用的比较多。

### 删除线

同样也有两个，分别为`<s></s>`和`<del></del>`,后面的有语义，用的多。

### 下划线

两个，分别为`<u></u>`和`<ins></ins>`,后面的有语义，为`insert`的缩写，用的较多。

简单的演示一下:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>网页标题</title>
    </head>
    <body>
        <strong>粗体</strong></br >
        <em>斜体</em></br >
        <del>删除线</del></br >
        <ins>下划线</ins></br >
    </body>
</html>
```

展示显示效果：

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html04.png"/>
</center>

## 图像标签

向网页中添加图片的方法为

```html
<img src="图片的路径">
```

这是一个单标签，`src`称为标签`<img>`的属性，它的值应该为图片的路径。

而路径又分为绝对路径和相对路径,下面将简单介绍一下二者，假设有如下的目录结构

```
磁盘C
    file1文件夹
        html.md
        p1.png
        image文件夹
            p2.png
    p3.png    
```

现在我打开的是html.md文件,如果我要与绝对路径插入p1.png, p2.png, p3.png这三张图片，那么方法是这样的

```html
<img src="C:/file1/p1.png">
<img src="C:/file1/image/p2.png">
<img src="C:/p3.png">
```

如果以相对路径插入图片，那么图片相对于该文件的位置就很重要，下面演示如何以相对路径插入图片

```html
<img src="p1.png">
<img src="image/p2.png">
<img src="../p3.png">
```

p1.png和文件html.md处于同一级目录，所以直接写src="p1.png"即可，而p2.png位于image文件夹，该文件夹与html.md文件处于同一目录，所以写为src="image/p2.png",而p3.png位于上级目录，`..`表示上一级目录，所以写为src="../p3.png"。

图像标签除了有`src`属性外，还有其他的属性，如下

| 属性          | 功能                                           |
| :------------- | :---------------------------------------------- |
| alt           | 图像不能显示时的替换文本                       |
| title         | 鼠标悬停显示的内容                             |
| width, height | 宽度和高度，设置其中一个就可以，会等比例的缩放 |
| border        | 添加边框，其值为边框的粗细                     |

下面给一个示例写法:

```html
<img src="p1.png" width="60%" title="p1.png" border="1">
```

## 链接标签

我们碰到，当我们点击一句话就可以跳转到一个网页，或者点击一张图片跳转。那么这个就是用链接标签实现的。链接标签为:

```html
<a href="链接地址">跳转的文字或图片</a>
```

比如你<a href="https://www.baidu.com">点击这里</a>就会跳转到百度。比较需要注意的是，跳转到外部的网页要写成https://www.baidu.com，而不要写成www.baidu.com。 另外，注意这个跳转是在本页面打开的，而不是新打开一个页面，如果需要在新页面打开，则可以改变其属性target的值，如下

```html
<a href="https://www.baidu.com" target = "_blank">点击这里</a>
```

target有两种值可选，分别为\_self和\_blank,默认为\_self，即在本页面打开。除了可以跳转到外链，也可以在页面内进行跳转，你可以给一个标签设定一个id值,比如

```html
<img src="1.png" id = "1">
```

我给这张插入的图片设了id值为1,那么我可以通过下面的语句跳转到这张图片

```html
<a href="#1">跳转到1.png</a>
```

通过id跳转前面要加\#号。

## 列表

### 无序列表

该种列表在实际中使用的较多,效果大概是这样

<ul>
    <li>足球</li>
    <li>篮球</li>
</ul>

无序列表的标签为

```html
<ul>
    <li>足球</li>
    <li>篮球</li>
</ul>
```

注意:`<ul></ul>`标签里面只能放`<li></li>`标签，而`<li><\li>`能放任何内容。

### 有序列表

有序列表也经常用，其标签与无序的很相似

```html
<ol>
    <li>足球</li>
    <li>篮球</li>
</ol>
```

效果是这样

<ol>
    <li>足球</li>
    <li>篮球</li>
</ol>

注意事项同无序。

### 自定义列表

使用以下格式定义自定义列表

```html
<dl>
    <dt>球类</dt>
    <dd>足球</dd>
    <dd>篮球</dd>
</dl>
```

效果如下

<dl>
    <dt>球类</dt>
    <dd>足球</dd>
    <dd>篮球</dd>
</dl>

## 表格

表格在网页中也很常见，其写法为

```html
<table>
    <tr>
        <td>学科</td>
        <td>分数</td>
    </tr>
    <tr>
        <td>物理</td>
        <td>99</td>
    </tr>
</table>
```

其效果为

<table>
    <tr>
        <td>学科</td>
        <td>分数</td>
    </tr>
    <tr>
        <td>物理</td>
        <td>99</td>
    </tr>
</table>

`<table></table>`标签就表示表格，而`<tr></tr>`表示一行，`<td></td>`表示一个单元格。table标签有很多的属性，如下

| 属性          | 功能                                                |
| ------------- | --------------------------------------------------- |
| align       | 该属性设定对齐方式，有left,center,right三个值可选 |
| cellspacing | 单元格与单元格之间的距离，默认为2                 |
| cellpadding | 单元格内的字与单元格之间的距离，默认为1           |

一般我们设置table的以下三个属性为0，这三个属性分别为cellspacing, cellpadding, border，称之为"三参为0"。我们还可使用`<caption></caption>`标签设置表格的标题，如

```html
<table>
    <caption>成绩单</caption>
    <tr>
        <td>学科</td>
        <td>分数</td>
    </tr>
    <tr>
        <td>物理</td>
        <td>99</td>
    </tr>
</table>
```

<table>
    <caption>成绩单</caption>
    <tr>
        <td>学科</td>
        <td>分数</td>
    </tr>
    <tr>
        <td>物理</td>
        <td>99</td>
    </tr>
</table>

因为表头比较特殊，所以有的时候我们在表头中不用td，使用th,该标签会加粗居中，我们改动如下

```
<th>学科</th>
<th>分数</th>
```

效果为

<table>
    <caption>成绩单</caption>
    <tr>
        <th>学科</th>
        <th>分数</th>
    </tr>
    <tr>
        <td>物理</td>
        <td>99</td>
    </tr>
</table>

下面讲的这个在实际中也经常遇到的是单元格合并。假如有下面这个表格

```html
<table>
    <tr>
        <td>123</td>
        <td>123</td>
        <td>123</td>
    </tr>
    <tr>
        <td>123</td>
        <td>123</td>
        <td>123</td>
    </tr>
    <tr>
        <td>123</td>
        <td>123</td>
        <td>123</td>
    </tr>
</table>
```

<table>
    <tr>
        <td>123</td>
        <td>123</td>
        <td>123</td>
    </tr>
    <tr>
        <td>123</td>
        <td>123</td>
        <td>123</td>
    </tr>
    <tr>
        <td>123</td>
        <td>123</td>
        <td>123</td>
    </tr>
</table>

我希望将最后一行的后两列合并成一个单元格,首先我们要知道单元格的合并分为行和并rowspan和列合并colspan,行合并是自左而右的，列合并是自上而下的。将最后一行的后两列合并为一个，所以是列和并，并且是两个合并成一个，所以colspan = "2",所以上面的程序改为

```html
<table>
    <tr>
        <td>123</td>
        <td>123</td>
        <td>123</td>
    </tr>
    <tr>
        <td>123</td>
        <td>123</td>
        <td>123</td>
    </tr>
    <tr>
        <td>123</td>
        <td colspan="2">123</td>
    </tr>
</table>
```

效果为

<table>
    <tr>
        <td>123</td>
        <td>123</td>
        <td>123</td>
    </tr>
    <tr>
        <td>123</td>
        <td>123</td>
        <td>123</td>
    </tr>
    <tr>
        <td>123</td>
        <td colspan="2">123</td>
    </tr>
</table>

在第三个tr(代表第三行)的第二个td处设置colspan="2",因为要合并最后一列，所以要删去最后一个td。

## 表单

现实中会经常提交表单，比如在注册邮箱时，你就需要提交表单，表单由三部分组成，表单控件，提示文本和表单域。

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html05.png"/>
</center>

现在重点介绍表单控件，因为这个包含了具体的表单功能项，如单行文本输入框、密码输入框、复选框、提交按钮、重置按钮等。与表单有关的标签是`<input/ >`,`<input/ >`是一个单标签，根据其属性type设置不同的值，可以指定不同的控件类型，如:

| type的值   | 控件               |
| ---------- | ------------------ |
| text     | 单行文本输入框     |
| password | 密码输入框         |
| radio    | 单选按钮           |
| checkbox | 复选框             |
| button   | 普通按钮           |
| submit   | 提交按钮           |
| reset    | 重置按钮           |
| image    | 图像形式的提交按钮 |
| file     | 文件域             |



`<input/ >`还有其他的属性配合type属性使用，如下

| 属性        | 功能                                 |
| ----------- | ------------------------------------ |
| name      | 控件的名称                           |
| value     | 控件的默认文本值                     |
| size      | 控件在页面中的显示宽度，只能为正整数 |
| checked   | 定义选择控件默认被选中的项           |
| maxlength | 控件允许输入的最大字符数             |

这里我想说一下单选按钮，假设有两个单选按钮，比如男和女让你选，按道理只能选一个，如下

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
    </head>
    <body>
        <input type="radio">男<input type="radio">女
    </body>
</html>
```

你可以发现两个都可以选到，解决办法将二者的name属性设置为相同的，比如sex,相同的name代表他们是同一组的，对于单选按钮，同一组的只能选中一个。

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html06.gif"/>
</center>

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
    </head>
    <body>
        <input type="radio" name="sex">男<input type="radio" name="sex">女
    </body>
</html>
```

这个时候你只能选一个。

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html07.gif"/>
</center>

下面要介绍一个`<label></label>`标签，当label与一个表单控件绑定时，点击该label会获得该控件的输入焦点,那么如何与表单控件绑定呢?使用for属性,如下

```html
<label for="word">Sex</label>
<input type = "text" id = "word">
```

当你点击Sex,会自动获得输入焦点。效果如下:

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html08.gif"/>
</center>

text只能输入一行的文本，事实上，我们在网页中经常见可以输入多行文本的输入框，比如在我们发表评论时的输入框，多行输入的标签是textarea,使用方法如下:

```html
<textarea cols="每行中的字符数" rows="显示的行数">
  默认显示文本
</textarea>
```

效果如下

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html09.png"/>
</center>

最后一个要介绍的就是下拉菜单，下拉菜单的标签是

```html
<select>
    <option>北京</option>
    <option>上海</option>
    <option>青岛</option>
</select>
```

如下所示

<center>
<select>
    <option>北京</option>
    <option>上海</option>
    <option>青岛</option>
</select>
</center>

当option中设置selected属性为selected="selected",那么这个option是默认选中项。比如我要设置上海为默认选中项那么

```html
<select>
    <option>北京</option>
    <option selected="selected">上海</option>
    <option>青岛</option>
</select>
```

<center>
<select>
    <option>北京</option>
    <option selected="selected">上海</option>
    <option>青岛</option>
</select>
</center>
