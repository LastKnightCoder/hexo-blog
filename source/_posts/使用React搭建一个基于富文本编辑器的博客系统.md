---
title: 使用React搭建一个基于富文本编辑器的博客系统
description: 介绍了从零搭建一个基于富文本编辑器的博客系统
categories: 
  - React
tags:
  - React 
  - antd 
  - React-Hooks
date: 2020-02-11
---

本文将从0开始搭建一个基于富文本编辑器的博客系统，包括的内容有

- `React`的基础知识
- 富文本编辑器组件`BraftEditor`的使用及扩展
- `React Hooks`
- `Redux + React-Redux`
- `antd`表单的介绍及使用
- 登录权限控制

# 项目前的准备

首先需要下载`Node.js`和`npm`(下载`Node.js`自带`npm`)，下载完`Node.js`后，由于`npm`的速度较慢，可以考虑使用下面的命令下载`cnpm`

```shell
npm install cnpm -g
```

>**npm是什么，为什么会需要npm：**
>
>想要知道npm是什么，那就要先从共享代码说起，程序员自古以来就有社区文化，加入社区最大的好处之一是，你可以使用别人贡献的代码，你也可以贡献代码给别人用。
>
>前端是怎么共享代码的呢?
>
>在 GitHub 还没有兴起的年代，前端是通过网址来共享代码，比如你想使用 jQuery，那么你点击 jQuery 网站上提供的链接就可以下载 jQuery，放到自己的网站上使用，GItHub 兴起之后，社区中也有人使用 GitHub 的下载功能。
>
>当一个网站依赖的代码越来越多，程序员发现这是一件很麻烦的事情，比如你要下载BootStrap，你就必须下载jQuery，因为BootStrap依赖jQuery，所以你需要一个网站一个网站的去下代码。如果遇到依赖比较多的情况，这个库依赖另一个库，另一个库又依赖另一个库，如此当依赖关系十分复杂时，你根本不知道要下哪些库，这对程序员来说简直就是个灾难。
>
>有些程序员就受不了了，一个程序员叫 Isaac Z. Schlueter(以下简称Isaaz)给出一个解决方案：用一个工具把这些代码集中到一起来管理吧!这个工具就是他用 JavaScript(运行在Node.js上)写的 npm，全称是 Node Package Manager。
>
>NPM 的思路大概是这样的：
>
>1. 买个服务器作为代码仓库(registry)，在里面放所有需要被共享的代码
>2. 发邮件通知 jQuery、Bootstrap、Underscore等作者使用npm publish 把代码提交到registry上，分别取名 jquery、bootstrap和underscore(注意大小写)
>3. 社区里的其他人如果想使用这些代码，就把jquery、bootstrap和underscore写到package.json里，然后运行 npm install，npm就会帮他们下载代码
>4. 下载完的代码出现在 node_modules 目录里，可以随意使用了。
>
>这些可以被使用的代码被叫做「包」(package)，这就是 NPM名字的由来：Node Package(包) Manager(管理器)。
>
>但是npm叫别人这么干，别人不一定会这么干啊，所以npm是怎么火的呢?npm的发展是跟Node.js的发展相辅相成的。Node.js是由一个在德国工作的美国程序员Ryan Dahl写的。他写了Node.js，但是Node.js缺少一个包管理器，于是他和 npm的作者一拍即合、抱团取暖，最终Node.js内置了npm。
>
>后来的事情大家都知道，Node.js火了。随着Node.js的火爆，大家开始用 npm来共享JS代码了，于是jQuery作者也将 jQuery发布到npm了。所以现在，你可以使用npm install jquery来下载jQuery代码。
>
>现在用npm来分享代码已经成了前端的标配。

接下来下载搭建`React`项目的脚手架`create-react-app`

```shell
cnpm install create-react-app -g
```
>**什么是脚手架：**
随着前端工程化的概念越来越深入人心，脚手架的出现就是为减少重复性工作而引入的命令行工具，摆脱ctrl + c, ctrl + v，此话zenjiang? 现在新建一个前端项目，已经不是在html头部引入css，尾部引入js那么简单的事了，css都是采用Sass或则Less编写，在js中引入，然后动态构建注入到html中；除了学习基本的js，css语法和热门框架，还需要学习构建工具webpack，babel这些怎么配置，怎么起前端服务，怎么热更新；为了在编写过程中让编辑器帮我们查错以及更加规范，我们还需要引入ESlint；甚至，有些项目还需要引入单元测试(Jest)。对于一个刚入门的人来说，这无疑会让人望而却步。而前端脚手架的出现，就让事情简单化，一键命令，新建一个工程，再执行两个npm命令，跑起一个项目。在入门时，无需关注配置什么的，只需要开心的写代码；另外，对于很多系统，他们的页面相似度非常高，所以就可以基于一套模板来搭建，虽然是不同的人开发，但用脚手架来搭建，相同的项目结构与代码书写规范，是很利于项目的后期维护的；以上就是为什么脚手架存在的意义， 让项目从"搭建-开发-部署"更加快速以及规范。

接着使用create-react-app搭建一个React项目，这里的项目名称就命名为blog

```shell
create-react-app blog
```

使用该命令会在本地生成一个文件夹blog，里面的内容如下

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703203219.png" width="50%"/>
</center>


其中src就是我们开发的文件夹，其中index.js是入口文件，里面的内容为

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(<App />, document.getElementById('root'));

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
```

现在我们将里面的内容改为

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(<div>Hello React</div>, document.getElementById('root'));
```

然后npm start就可以看到下面的页面

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703203713.png" width="80%"/>
</center>

这时可以将src下除index.js的文件全部都删掉了，因为没有用到他们。

# React基础

## JSX语法

可能现在你对上面的代码不太了解，特别是怎么在JavaScrpt里面写HTML

```jsx
ReactDOM.render(<div>Hello React</div>, document.getElementById('root'));
```

这里的HTML就是JSX，JSX的全称是JavaScript XML，指的就是JavaScript中的XML。

上面这行语句的作用是什么呢? React会解析这个JSX语句为一个虚拟DOM，而ReactDOM会将这个虚拟DOM转变为真正的DOM，然后塞到页面某个特定的元素上面，这里是塞到一个id为root(该DOM元素在public/index.html中)的DOM对象中,我们查看index.html中的内容

```html
<!DOCTYPE html>
<html lang="en">
  <!-- 省略head中的内容和所有注释 -->
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

经过ReactDOM渲染后的index.html的页面应该为为

```html
<!DOCTYPE html>
<html lang="en">
  <!-- 省略head中的内容和所有注释 -->
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root">
      <div>Hello React</div>
    </div>
  </body>
</html>
```

为了验证这个想法，可以在刚才的页面中按下F12检查元素

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703203853.png" width="50%"/>
</center>

所以页面显示出来的就是Hello React。

>**为什么需要虚拟DOM：**
>
>原因很简单，那就是DOM很慢，我们先来看一下浏览器的渲染流程
> 
> - 用HTML分析器，分析HTML元素，构建一颗DOM树
> - 用CSS分析器，分析CSS文件和元素上的inline样式，生成页面的样式表
> - 将上面的DOM树和样式表，关联起来，构建一颗Render树。这一过程又称为Attachment。每个DOM节点都有attach方法，接受样式信息，返回一个render对象（又名renderer）。这些render对象最终会被构建成一颗Render树
> - 有了Render树后，浏览器开始布局，会为每个Render树上的节点确定一个在显示屏上出现的精确坐标值
> - Render数有了，节点显示的位置坐标也有了，最后就是调用每个节点的paint方法，让它们显示出来
>
> 当你用传统的原生api或jQuery去操作DOM时，浏览器会从构建DOM树开始从头到尾执行一遍流程。比如当你在一次操作时，需要更新10个DOM节点，理想状态是一次性构建完DOM树，再执行后续操作。但浏览器没这么智能，收到第一个更新DOM请求后，并不知道后续还有9次更新操作，因此会马上执行流程，最终执行10次流程。显然例如计算DOM节点的坐标值等都是白白浪费性能，可能这次计算完，紧接着的下一个DOM更新请求，这个节点的坐标值就变了，前面的一次计算是无用功。
>
>即使计算机硬件一直在更新迭代，操作DOM的代价仍旧是昂贵的，频繁操作还是会出现页面卡顿，影响用户的体验。
>
>虚拟DOM就是为了解决这个浏览器性能问题而被设计出来的。例如前面的例子，假如一次操作中有10次更新DOM的动作，虚拟DOM不会立即操作DOM，而是将这10次更新的diff内容保存到本地的一个js对象中，最终将这个js对象一次性attach到DOM树上，通知浏览器去执行绘制工作，这样可以避免大量的无谓的计算量。

我们知道React中的JSX会被解析为一个React对象(或者说JavaScript对象)，所以我们在写JSX的过程中，就可以把JSX看做是一个对象(记住JSX的本质就是对象，每当在JavaScript代码中看到这种JSX结构的时候，脑子里面就可以自动做转化，这样对你理解React.js的组件写法很有好处)。这就意味着JSX只能有一个根元素，如下面的写法是错误的

```jsx
<div></div>
<div></div>
```

因为上面的代码并不能被解析为一个对象，正确的写法应该是

```jsx
<div>
    <div></div>
    <div></div>
</div>
```

这样的JSX才会被解析为一个对象。

现在我们在来说一下文件开头的两行语句

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
```

在React中，我们使用import来导入npm下载的包，这里我们引入了React和ReactDOM，这里你可能有疑问，我们并没有使用npm下载react和react-dom，为什么能够引入，这是因为create-react-app为我们做了这件事情，打开package.json我们可以发现已经帮我们添加了react和react-dom

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703204024.png" width="50%"/>
</center>

那么引入的React是做什么事情的呢? 首先在JavaScript中是不能写JSX的，在React中的JSX会被解析为JavaScript对象，就是通过React来做到的，而ReactDOM是将React渲染的JSX对象(有时我们也称之为组件)转变为真正的DOM对象并塞到某个特定的元素里面。所以记住只要你写React.js组件，都必须要引入React。

## 组件

这里十分推荐[React.js 小书](http://huziketang.mangojuice.top/books/react/)，里面将React的内容讲的十分的深入浅出，每读一遍都有新的收获。

组件是什么? 如果你搭过积木的话，那么里面的积木就是组件，我们使用积木搭建出一个东西，在React中我们使用组件来搭建一个页面。组件的作用就是复用，这里的复用指的不仅仅是页面的复用，还有逻辑和样式；除此以外，有的组件并不是用来展现页面的，而是用来加载数据的，然后将数据传给子组件，这样会使得处理数据的逻辑和展示数据的逻辑分开，职责分明，逻辑清晰，以及便于后期的维护。另外，有的组件是做权限的认证，以决定展示某些页面与否。总而言之，有许多不同的组件，组件根据功能或写法或有无状态等等可以进行分类，在后面将详细讨论。

按照组件的写法可以分为函数组件和类组件。

### 类组件

我们写一个类组件HelloReact

<iframe height="302" style="width: 100%;" scrolling="no" title="React类组件使用" src="https://codepen.io/lastknightcoder/embed/LYVEeEE?height=302&theme-id=default&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lastknightcoder/pen/LYVEeEE'>React类组件使用</a> by LastKnightCoder
  (<a href='https://codepen.io/lastknightcoder'>@lastknightcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

使用类组件，里面必须有一个render函数，该方法是用来返回JSX代码。我们创建了一个HelloWorld组件

```jsx
ReactDOM.render(<HelloReact />, document.getElementById("root"))
```

并且我们通过`<HelloReact />`的方式使用了组件，此时的`<HelloReact />`就相当于render函数的返回值

```jsx
<div>Hello React</div>
```

但`<HelloReact />`不仅仅是HTML，虽然在这个组件中，我们并没有加上JavaScript的逻辑和CSS样式，但是我们知道一个组件是包括这些的，下面我们为这个类添加样式和点击函数

<iframe height="265" style="width: 100%;" scrolling="no" title="abOzEWb" src="https://codepen.io/lastknightcoder/embed/abOzEWb?height=265&theme-id=default&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lastknightcoder/pen/abOzEWb'>abOzEWb</a> by LastKnightCoder
  (<a href='https://codepen.io/lastknightcoder'>@lastknightcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

两点说明：

- 由于class在React中是关键字，所以类名要写成className
- onClick={()=> console.log("click me")}为div标签添加了点击事件，onClick={}并不是说onClick是一个对象，{}是React的插值语法，使用插值语法可以将组件的属性与变量(表达式)绑定起来，这样就不会写死，而是会根据变量取不同的值。这里就是将onClick这个React事件与{}里面的箭头函数绑定起来。**在{}中的内容只能是JavaScript的表达式，不能是if，while等语句**


>**React事件：**
>
>使用原生的JS为DOM元素添加事件，主要有下面三种方法
>
>- 在HTML中直接事件绑定
>
>  ```html
>  <p onclick="console.log('click')"></p>
>  ```
>
>- 直接绑定
>
>  ```javascript
>  let divObj = document.getElementById("root");
>  divObj.onclick = function() {}
>  ```
>
>- 对DOM对象进行事件监听处理,事件委托监听方式
>
>  ```javascript
>  let divObj = document.getElementById("root");
>  divObj.addEventListener('click', function(){});
>  ```
>
>至于这三种方法的优缺点与及什么时候使用，这里却不在讨论。那么React中的事件是如何绑定的呢?
>
>在React中事件的绑定是直接写在JSX元素上的,不需要通过addEventListener事件委托的方式进行监听，写法上:
>
>- 在JSX元素上添加事件,通过on*EventType这种内联方式添加,命名采用小驼峰式(camelCase)的形式,而不是纯小写(原生HTML中对DOM元素绑定事件,事件类型是小写的),无需调用addEventListener进行事件监听,也无需考虑兼容性,React已经封装好了一些的事件类型属性(ps:onClick,onMouseMove,onChange,onFocus)等
>
>- 使用JSX语法时,需要传入一个函数作为事件处理函数,而不是一个字符串,也就是props值应该是一个函数类型数据,事件函数方法外面得用一个双大括号包裹起来(也就是上面提到的插值语法)
>
>- on*EventType的事件类型属性,只能用作在普通的原生html标签上(例如:div,input,a,p等,例如:`<div onClick={ 事件处理函数 }></div>`),无法直接用在自定义组件标签上,也就是说下面这么写是没有作用的
>
>  ```javascript
>  <HelloReact onClick={()=> console.log("click me")}>
>  ```
>
>- 不能通过返回false的方式阻止默认行为,必须显示使用preventDefault
>
>具体React事件有哪些，可以参照[事件系统|React](https://react-cn.github.io/react/docs/events.html)。


#### props

组件最常用的作用就是用来展示数据，那么作为可复用的组件，不同人拿到这个组件所展示的数据也不会相同，那么数据从哪里来? 使用组件的人怎么将数据传给组件，而组件又怎么拿到数据。这就要用到props了。

假如有下面的组件

```jsx
class DisplayData extends React.Component {
    render() {
        return (
            <div>假设这里是要展示的数据</div>
        )
    }
}
```

这个组件的作用就是展示数据，现在我要用DisplayData组件，我怎么把数据传过去，如下

```jsx
<DisplayData data="数据" />
```

我们通过给DisplayData组件加上一个data属性，值是"数据"，就可以将数据传给DisplayData，现在怎么拿到数据呢? 每一个组件都有props属性，它是一个对象，通过上面方法传递给组件的属性都会以键值对的形式添加到props对象中，所以在DisplayData组件中，我们就可以通过this.props.data拿到数据

```jsx
class DisplayData extends React.Component {
    render() {
        return (
            <div>{this.props.data}</div>
        )
    }
}
```

<iframe height="265" style="width: 100%;" scrolling="no" title="React props父组件给子组件传递数据" src="https://codepen.io/lastknightcoder/embed/GRJgGvY?height=265&theme-id=default&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lastknightcoder/pen/GRJgGvY'>React props父组件给子组件传递数据</a> by LastKnightCoder
  (<a href='https://codepen.io/lastknightcoder'>@lastknightcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

以上我们把DisplayData称之为子组件，而使用DisplayData的组件称之为父组件，上面演示了如何将数据从父组件传给子组件，现在我们考虑一下，如何将子组件的数据传递到父组件呢? 完全是有这个需要的，比如子组件是一个登录组件，我们需要将用户输入的用户名和密码交由父组件进行处理(为什么要交给父组件处理，在子组件中处理不可以吗?如果你在子组件里面处理了数据，那么这个组件就与具体的业务相关了，就不能够被复用了，所以数据处理的工作需要交由父组件来做)。

其实也很简单，我们给子组件传递一个回调函数，那么在子组件的某个时刻(比如子组件的值改变了或者说点击提交)时调用此回调函数并传入数据，这样我们就可以在父组件中拿到数据了

<iframe height="620" style="width: 100%;" scrolling="no" title="React子组件向父组件传递数据" src="https://codepen.io/lastknightcoder/embed/eYNmKMB?height=620&theme-id=dark&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lastknightcoder/pen/eYNmKMB'>React子组件向父组件传递数据</a> by LastKnightCoder
  (<a href='https://codepen.io/lastknightcoder'>@lastknightcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

上面的逻辑代码为当SonComponent中的input发生改变时(输入内容或删减内容等等)，会触发子组件中的handleValue回调函数(该回调函数bind了this，后面会进行解释)，在子组件的handleValue中，我们根据浏览器传入的event获得了input输入框的值，并且调用ParentComponent传入的handleValue回调函数将此值传入，这样就将子组件的数据传给了父组件，在父组件中，我们定义了这个回调函数接收这个值，并在控制台打印，你可以在上面的input中输入值并在控制台观察结果。

**JavaScript函数里面的this是什么：**

要想知道JavaScript函数里面的this是什么，就要知道函数调用的4种方式：

- 定义在全局作用域中函数

定义在全局作用域中的函数，根据是否是严格模式下，this的取值也不同，如果是在严格模式下，里面的this是undefined，如果是在非严格模式下，里面的this是window

```javascript
"use strict"
function test() {
    console.log(this); 
}
test(); //undefined
```
```javascript
function test() {
    console.log(this); 
}
test(); //window
```

- 对象方法

对象中方法中的this如果是对象.的形式调用的，那么对象方法中的this就是该对象

```javascript
let obj = {
    test: function() {
        console.log(this);
    }
}
obj.test(); // obj
```

如果以某种角度看的话，第一种情况的非严格模式下是第二种情况的特例，我们知道在全局作用域下声明的变量和函数都会成为window对象的属性，当我们在全局作用域下声明一个test函数，就相当于在window对象中添加了一个test方法(在对象中的函数我们一般称为方法)，而调用test()方法就相当于window.test()，按照第二种情况，test中的this就是应该指向window

- 作为构造函数被调用

当我们new一个方法的时候，里面的this是一个空对象

```javascript
function Dog() {
    console.log(this)
}
new Dog(); // {}
```

- 使用call, apply, bind方法改变函数的上下文(this)

上面三者都可以改变函数执行时内部this的指向，下面来看一个例子

```javascript
funtion printName(firstName, lastName) {
    console.log(this.fullName)
    console.log(`${firstName} ${lastName}`)
}
```

如果直接执行这个函数的话，那么this.fullName的结果就是undefined，因为window对象没有这个属性，但是如果有以下对象

```javascript
let obj = {
    fullName: "David"
}
```

下面我们将printName函数执行时内部的this指向obj

```javascript
let firstName = "firstName"
let lastName = "lastName"
// 将printName内部this指向obj， 后面是printName需要的参数
printName.apply(obj, firstName, lastName) 
// 将printName内部this指向obj， 后面是printName需要的参数
printName.call(obj, [firstName, lastName])
```

这时this.fullName就是obj中的fullName了，因为apply和call方法改变了printName内的this指向。这里我们发现apply和call方法是极其的相似，除了传递参数时格式不一样；事实上也是如此，apply和call的功能是一样的。

说完apply和call，接下来讲一讲bind，bind与上面两者不同，上面改变函数内部的this指向时是立即执行这个函数的，而使用bind改变函数内部的this指向时，这个函数不会立即的执行，如

```javascript
printName = printName.bind(obj)
```

printNAme函数内部的this指向已经改变，当printName执行时，打印出的this.fullName就是obj里面的David。


了解完JavaScript中的this是什么，接下来就要解释

```jsx
<input type="text" placeholder="Please Input" onChange={this.handleValue.bind(this)} />
```

中onChange={this.handleValue.bind(this)}，首先我们来看handleValue中的代码

```jsx
handleValue(event) {
    this.props.handleValue(event.target.value)
}
```

当input发生改变时便会执行这个函数，但是是谁执行这个函数呢? 是window，而React是运行在严格模式下的，所以这时的this就是undefined，所以我们拿不到我们想要的this，要使得我能拿到的使我们想拿到的this，就是改变函数执行时内部的this指向，考虑到这个函数是作为回调函数而不是立即执行，我们使用bind来绑定this。

机智的你已经发现，ParentComponent中的handleValue没有bind(this)，这是因为它在函数里面没有用到this啊，所以什么时候bind(this)是不是已经很清楚了呢!

到这里你有没有发现我们使用组件都是这样

```jsx
<HelloReact />
```

居然不是一开一闭的格式，那想必你有疑问，可不可以这样使用

```jsx
<HelloReact></HelloReact>
```

答案是可以，那么问题来了? 二者又有什么不同? 使用后面的写法意味着可以在标签里面写子元素，如

```jsx
<HelloReact>
    <div>inner</div>
</HelloReact>
```

其实这也可以看做是一种传递数据的方式，标签里面的子元素会传给这个组件，传过去的数据会保存在props.children中，在HelloReact中可以通过this.props.children获得数据。

这两种写法都很常见，在写布局，路由，认证等组件时经常使用后面的写法，而在一些展示的组件中，通常只需要父组件传下来的数据，会使用前一种写法，当然这种情况下也可以使用第二种写法。

为了理解children的应用，我们来写简单的Layout组件。所谓的Layout，就是布局

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703204311.png" width="80%"/>
</center>

我们简单的把页面上面三个部分，这就是一种布局，一个头部，一个侧边栏，一个内容区。很多时候我们发现一个网站的多个网页之间的布局是一样的，并且很有可能头部和侧边栏是相同，仅仅是内容区不同，作为一名优秀的程序员，当然要尽可能的抽离出这些重复的代码，我们把这个布局抽离为一个组件

<iframe height="446" style="width: 100%;" scrolling="no" title="尝试Layout" src="https://codepen.io/lastknightcoder/embed/MWwYqEL?height=446&theme-id=dark&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lastknightcoder/pen/MWwYqEL'>尝试Layout</a> by LastKnightCoder
  (<a href='https://codepen.io/lastknightcoder'>@lastknightcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

首先希望不要关注样式，因为那不是重点，关注Layout的结构。注意我们将{this.props.children}放在了类名为content的div中，所以如果我们将Content内容区组件放到Layout里面，Content组件就会被放在Layout的内容区，放置不同的Content组件，就会得到多个布局一样，内容不同的页面，这就做到了复用。

整个网站当然不可能只会有一种布局，很多时候我们会写多个布局的组件，明白了布局组件的作用，这些对你来说应当不难。

#### state

React中一个比较重要的思想就数据驱动视图，例如子组件根据props的内容进行展示，根据不同的props展示不同的内容，不同的props会展现不同的UI，所以可以认为是props决定了视图，每当props变化时，都会引起子组件的渲染，展现不同的视图，视图的改变完全在于数据，所以现在我们操作的重点不再是DOM，而是数据，我们通过操作数据来达到不同的UI效果。

但是仅仅靠props似乎是够的，因为props是只读的，它不能够更改。这意味着什么? 假设父组件传给子组件的props没有发生变化，那么子组件的视图就不会改变，因为数据没有改变。这意味着我们如何与这样一个组件进行交互呢?

这意味组件的内部需要数据来管理UI的变化，我们将组件内部的数据称之为state，state就是状态的意思。比如使用visible这个状态来控制某个对话框是否可见，用户通过改变visible这个状态从而影响组件UI的变化，每次state的变化都会引起组件UI的刷新，从而达到数据驱动视图的目的。从此，我们关心的再也不是DOM操作，而是props, state这些数据，我们操作这些数据来控制视图的更新。

定义一个合适的state，是正确创建组件的第一步。state必须能代表一个组件UI呈现的完整状态集，即组件的任何UI改变，都可以从state的变化中反映出来；同时，state还必须是代表一个组件UI呈现的最小状态集，即state中的所有状态都是用于反映组件UI的变化，没有任何多余的状态，也不需要通过其他状态计算而来的中间状态。

组件中用到的一个变量是不是应该作为组件state，可以通过下面的4条依据进行判断：

- 这个变量是否是通过props从父组件中获取? 如果是，那么它不是一个状态。
- 这个变量是否在组件的整个**生命周期**(后面讲到)中都保持不变? 如果是，那么它不是一个状态。
- 这个变量是否可以通过其他状态(state)或者属性(props)计算得到?如果是，那么它不是一个状态。
- 这个变量是否在组件的render方法中使用? 如果不是，那么它不是一个状态(因为state是用来驱动视图的，如果这个变量没有在render方法中使用，意味着该状态的改变并不能使得视图发生变化)。这种情况下，这个变量更适合定义为组件的一个普通属性，例如组件中用到的定时器，就应该直接定义为this.timer，而不是this.state.timer。

现在来写一个计数器，当点击按钮时页面上显示的数字+1，页面上的数字发生改变正是UI的改变，这个时候我们就要用state来管理页面上要展示的数据

<iframe height="265" style="width: 100%;" scrolling="no" title="Counter演示state" src="https://codepen.io/lastknightcoder/embed/YzXPmjZ?height=265&theme-id=dark&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lastknightcoder/pen/YzXPmjZ'>Counter演示state</a> by LastKnightCoder
  (<a href='https://codepen.io/lastknightcoder'>@lastknightcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

我们在constructor构造函数中初始化state为

```jsx
this.state = {
  number: 0
}
```

state为一个对象，其中的number属性正是我们要展现的数据，我们将它初始化为0。当我们点击按钮时，触发状态的改变

```jsx
increment() {
  this.setState({
    number: ++this.state.number
  })
}
```

注意，状态的改变不能使用this.state.number = ++this.state.number使状态发生改变，要使得状态发生改变，必须使用setState()方法使状态发生改变。

上面的例子进一步验证了数据驱动视图的思想，在计数器的例子中，我们没有手动的更改视图(操作DOM)，而是通过改变state来使得视图得到刷新，因为每次state的改变都会引起render()方法的调用，从而使得视图得以刷新。

请务必牢记，并不是组件中用到的所有变量都是组件的状态! 当存在多个组件共同依赖一个状态时，一般的做法是状态上移，将这个状态放到这几个组件的公共父组件中。

所谓状态上移是怎么回事呢? 考虑下面这么一个评论组件

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703204445.png"/>
</center>

CommentInput用来输入评论，当提交之后会在CommentList中新增一个CommentItem来显示新增的评论，很明显我们需要一个comments数组，当CommentInput提交评论时，将comment添加到comments数组中，CommentList拿到comments数组，comments中的元素会交给其中的CommentItem显示。而作为关键的comments数组，它是多个组件都会用到的，所以它应该作为state保存在公共的父组件中，即Comment组件中。这就是状态上移，将多个组件都会用到的数据上移到共同父组件的state中，当更新父组件的state的时，会使得传到子组件的props相应的更新，从而达到视图 更新的目的

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703204538.png" width="80%"/>
</center>

#### 生命周期函数

一个人从出生到死亡会经历一些人生节点，孩童、青年、中年、老年。而一个组件从创建到销毁也要经历特殊的节点，在这些节点中，我们可以做一些操作，每一个节点都对应一个函数，我们将它们称为生命周期函数。

那React有哪些生命周期函数，这些生命周期函数对应的节点是什么呢? 或者说这些生命周期函数在React从创建到销毁的哪一个过程会被执行呢? 这里我们将生命周期分为三个阶段：

- 创建阶段
- 更新阶段
- 卸载阶段

在每一个阶段都包含数个生命周期函数。

##### 创建阶段

###### constructor(props)

我们一般在构造函数中干两件事情：

- 初始化状态
- 为事件处理函数绑定this

>**注意：** ES6子类的构造函数必须执行一次super()。React如果构造函数中要使用this.props，必须先执行super(props)。

###### static getDerivedStateFromProps(nextProps, prevState)

当创建时、接收新的 props 时、setState时、forceUpdate时会执行这个方法。这是一个静态方法，参数nextProps是新接收的props，prevState是当前的state。返回值(对象)将用于更新state，如果不需要更新则需要返回null。

这个方法在建议尽量少用，只在必要的场景中使用，一般使用场景如下：

- 无条件的根据props更新state
- 当props和state的不匹配情况更新state

###### componentWillMount()

这个方法已经不推荐使用。因为在未来异步渲染机制下，该方法可能会多次调用。它所行使的功能也可以由 componentDidMount() 和 constructor() 代替：

- 之前有些人会把异步请求放在这个生命周期，其实大部分情况下都推荐把异步数据请求放在 componentDidMount() 中。
- 在服务端渲染时，通常使用 componentWillMount() 获取必要的同步数据，但是可以使用 constructor() 代替它。

>有定义getDerivedStateFromProps时，会忽略componentWillMount()

###### render()

每个类组件中，render() 唯一必须的方法。render() 正如其名，作为渲染用，可以返回下面几种类型：

- React 元素（React elements）
- 数组（Arrays）
- 片段（fragments）
- 插槽（Portals）
- 字符串或数字（String and numbers）
- 布尔值或 null（Booleans or null）

里面不应该包含副作用，应该作为纯函数。**不能使用 setState。**

###### componentDidMount()

组件完成装载（已经插入 DOM 树）时，触发该方法。这个阶段已经获取到真实的 DOM。一般用于下面的场景：

- 异步请求 ajax
- 添加事件绑定(注意在componentWillUnmount中取消，以免造成内存泄漏)

##### 更新阶段

###### componentWillReceiveProps()

这个方法在接收新的 props 时触发，即使 props 没有变化也会触发。一般用这个方法来判断 props 的前后变化来更新 state，如下面的例子：

```jsx
class ExampleComponent extends React.Component {
  state = {
    isScrollingDown: false,
  };
 
  componentWillReceiveProps(nextProps) {
    if (this.props.currentRow !== nextProps.currentRow) {
      this.setState({
        isScrollingDown:
          nextProps.currentRow > this.props.currentRow,
      });
    }
  }
}
```

这个方法将被弃用，推荐使用 getDerivedStateFromProps 代替。

###### static getDerivedStateFromProps()

见创建阶段的描述

###### shouldComponentUpdate()

在接收新的 props 或新的 state 时，在渲染前会触发该方法。该方法通过返回 true 或者 false 来确定是否需要触发新的渲染。返回 false， 则不会触发后续的 componentWillUpdate()、render() 和 componentDidUpdate()（但是 state 变化还是可能引起子组件重新渲染）。

所以通常通过这个方法对 props 和 state 做比较，从而避免一些不必要的渲染。

###### componentWillUpdate()

当接收到新的 props 或 state 时，在渲染前执行该方法。在以后异步渲染时，可能会出现某些组件暂缓更新，导致 componentWillUpdate 和 componentDidUpdate 之间的时间变长，这个过程中可能发生一些变化，比如用户行为导致 DOM 发生了新的变化，这时在 componentWillUpdate 获取的信息可能就不可靠了。这个方法将要弃用。

###### render()

见创建阶段的描述

###### getSnapshotBeforeUpdate()

这个方法在 render() 之后，componentDidUpdate() 之前调用。两个参数 prevProps 表示更新前的 props，prevState 表示更新前的 state。返回值称为一个快照（snapshot），如果不需要 snapshot，则必须显示的返回 null —— 因为返回值将作为 componentDidUpdate() 的第三个参数使用。所以这个函数必须要配合 componentDidUpdate() 一起使用。

这个函数的作用是在真实 DOM 更新（componentDidUpdate）前，获取一些需要的信息（类似快照功能），然后作为参数传给 componentDidUpdate。例如：在 getSnapShotBeforeUpdate 中获取滚动位置，然后作为参数传给 componentDidUpdate，就可以直接在渲染真实的 DOM 时就滚动到需要的位置。

下面是官方文档给出的例子：

```jsx
class ScrollingList extends React.Component {
  constructor(props) {
    super(props);
    this.listRef = React.createRef();
  }
 
  getSnapshotBeforeUpdate(prevProps, prevState) {
    // Are we adding new items to the list?
    // Capture the scroll position so we can adjust scroll later.
    if (prevProps.list.length < this.props.list.length) {
      const list = this.listRef.current;
      return list.scrollHeight - list.scrollTop;
    }
    return null;
  }
 
  componentDidUpdate(prevProps, prevState, snapshot) {
    // If we have a snapshot value, we've just added new items.
    // Adjust scroll so these new items don't push the old ones out of view.
    // (snapshot here is the value returned from getSnapshotBeforeUpdate)
    if (snapshot !== null) {
      const list = this.listRef.current;
      list.scrollTop = list.scrollHeight - snapshot;
    }
  }
 
  render() {
    return (
      <div ref={this.listRef}>{/* ...contents... */}</div>
    );
  }
}
```

###### componentDidUpdate()

这个方法是在更新完成之后调用，第三个参数 snapshot 就是 getSnapshotBeforeUpdate 的返回值。

正如前面所说，有 getSnapshotBeforeUpdate 时，必须要有 componentDidUpdate。所以这个方法的一个应用场景就是上面看到的例子，配合 getSnapshotBeforeUpdate 使用。

##### 卸载阶段

componentWillUnmount()

在组件卸载或者销毁前调用。这个方法主要用来做一些清理工作，例如：

- 取消定时器
- 取消事件绑定
- 取消网络请求

上面三个阶段是正常的生命周期，但是如果发生了异常，就需要进行错误处理，所以React也提供了发生异常时的函数

##### 发生异常

componentDidCatch(err, info)

任何子组件在渲染期间，生命周期方法中或者构造函数 constructor 发生错误时调用。

错误边界不会捕获下面的错误：

- 事件处理(Event handlers)(因为事件处理不发生在 React 渲染时，报错不影响渲染)
- 异步代码(Asynchronous code)(e.g. setTimeout or requestAnimationFrame callbacks)
- 服务端渲染(Server side rendering)
- 错误边界本身(而不是子组件)抛出的错误

下面我们来看一张[生命周期函数图](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)来加深对上面描述的生命周期函数的理解

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200211010936.png"/>
</center>
虽然React有做向下兼容，但是推荐尽量避免使用废弃的生命周期，而是拥抱未来，用新的生命周期替换它们。

### 函数式组件

现在我们用函数式组件写一个HelloReact组件

<iframe height="265" style="width: 100%;" scrolling="no" title="函数式组件示例" src="https://codepen.io/lastknightcoder/embed/RwPPBPE?height=265&theme-id=dark&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lastknightcoder/pen/RwPPBPE'>函数式组件示例</a> by LastKnightCoder
  (<a href='https://codepen.io/lastknightcoder'>@lastknightcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

其中函数式组件接收一个参数props，通我们在类组件中介绍的props相同。函数式组件与类组件不同的是，函数式组件没有state和生命周期函数，所以函数式组件又被称为无状态组件，从某种意义上说类组件具有更加强大的功能。

为了创建纯展示组件，这种组件只负责根据传入的props来展示，不涉及到要state状态的操作。其官方指出：在大部分React代码中，大多数组件被写成无状态的组件，通过简单组合可以构建成其他的组件等；这种通过多个简单然后合并成一个大应用的设计模式被提倡。

无状态组件的创建形式使代码的可读性更好，并且减少了大量冗余的代码，精简至只有一个render方法，大大的增强了编写一个组件的便利，除此之外无状态组件还有以下几个显著的特点：

- 组件不会被实例化，整体渲染性能得到提升

因为组件被精简成一个render方法的函数来实现的，由于是无状态组件，所以无状态组件就不会在有组件实例化的过程，无实例化过程也就不需要分配多余的内存，从而性能得到一定的提升。

- 组件不能访问this对象

无状态组件由于没有实例化过程，所以无法访问组件this中的对象，例如：this.ref、this.state等均不能访问。若想访问就不能使用这种形式来创建组件

- 组件无法访问生命周期的方法

因为无状态组件是不需要组件生命周期管理和状态管理，所以底层实现这种形式的组件时是不会实现组件的生命周期方法。所以无状态组件是不能参与组件的各个生命周期管理的。

- 无状态组件只能访问输入的props，同样的props会得到同样的渲染结果，不会有副作用

无状态组件被鼓励在大型项目中尽可能以简单的写法来分割原本庞大的组件，未来React也会这种面向无状态组件在譬如无意义的检查和内存分配领域进行一系列优化，所以只要有可能，尽量使用无状态组件。

## 组件分类

组件按写法分可以分为类组件和函数式组件，在上面已经详细介绍过。按照组件有无状态又可以将组价分为有状态组件和无状态组件。按照组件的功能分又可以将组件分为容器组件和展示组件，又或者称为Smart组件和Dumb组件。这里主要介绍容器组件和展示组件。

展示组件：

- 只关注看起来怎么样
- 内部可能包含有展示组件或容器组件，并且通常有DOM标签和样式
- 不用指明数据是如何加载或者变化的
- 只通过props接收数据和回调函数
- 一般写为函数式组件，除非它们需要状态、生命周期函数或者性能优化

容器组件：

- 只关注如何工作的
- 内部也许包含展示组件或容器组件，但一般不会有DOM标签除非一个包装的div，不可能会有样式
- 为展示组件或其他容器组件提供数据和回调函数
- 一般是有状态的，被认为是数据源
- 一般是从高阶组件生成的

通过将组件分为这两种，可以有以下优点：

- 关注点更好的分离
- 更好的复用性

## 高阶组件

高阶组件指的不是组件，而是一个函数，它可以增强组件，它接收一个组件，返回一个新组件，这个新组件是接收组件的加强版。假设有这样三个组件

```jsx
class App1 extends React.Component {
  state = { data: null };
  componnetDidMount() {
    const data = localStorage.getItem("data");
    this.setState({
      data
    });
  }

  render() {
    return <div>{this.state.data}</div>;
  }
}

class App2 extends React.Component {
  state = { data: null };
  componnetDidMount() {
    const data = localStorage.getItem("data");
    this.setState({
      data
    });
  }

  render() {
    return <div>{this.state.data}</div>;
  }
}

class App3 extends React.Component {
  state = { data: null };
  componnetDidMount() {
    const data = localStorage.getItem("data");
    this.setState({
      data
    });
  }
  render() {
    return <div>{this.state.data}</div>;
  }
}
```

观察这三个组件，它们都有一个共同的特点，那就是它们在挂载后都要从localStorage中获取数据，可见这样的代码是重复了的，我们如果每次要从localStorage获取数据，都要写一遍这样的代码，那怎么可以让这样的逻辑复用呢? 这就是高阶组件的作用，考虑下面这样的一个函数

```jsx
function getDataFromLocalStorage(WrapComponent) {
  class NewComponent extends React.Component {
    state = { data: null };
    componentDidComponent() {
      const data = localStorage.getItem("data");
      this.setState({
        data
      });
    }
  
    render() {
      return <WrapComponent data={this.state.data} />
    }
  }

  return NewComponent
}
```

该函数接收一个组件，返回一个新组件。这个新组件在挂载后会用localStorage读取数据，并注入到传入的组件的props.data中。而这个传入的组件会作为新组件render函数的返回值，所以当我们使用这个新组件时，与使用传入的组件一致，不过此时新组件的props中已经有了从localStorage读取的数据。我们可以将上面的三个组件都可以改造为

```jsx
class App1 extends React.Component {
  render() {
    return <div>{this.props.data}</div>
  }
   
  App1 = getDataFromLocalStorage(App1)
}
        
class App2 extends React.Component {
  render() {
    return <div>{this.props.data}</div>
  }
   
  App2 = getDataFromLocalStorage(App2)
}

class App3 extends React.Component {
  render() {
    return <div>{this.props.data}</div>
  }
   
  App3 = getDataFromLocalStorage(App3)
}
```

我们使用高阶函数为三个组件的props.data中注入了数据，这样三个组件只要调用这个方法，就可以自动可以在props中获取到数据，从而达到了代码的复用。可能读到这里你还不能理解，没关系，多读几遍好好消化，或者写个几遍就习惯了。

## render props

render props的目的同样也是进行代码的复用。再次考虑上面代码的复用，不过这次使用render props实现代码的复用

```jsx
class GetDataFromLocalStorage extends React.Component {
  state = {data: null}
  componentDidMount() {
    this.setState(localStorage.getItem("data"))
  }

  render() {
    return {this.props.render(this.state)}
  }
}

class App extends React.Component {
  render() {
    return (
      <GetDataFromLocalStorage 
        render = {({data}) => {
          return <div>data</div>
        }} 
      />
    )
  }
}
```

仔细阅读上面的代码，想必还是比较容易读懂的。Render Props的核心思想是，通过一个函数将class组件的state作为props传递给纯函数组件。

# React Redux

假设有这么一棵组件树

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703204718.png"/>
</center>

Com4从App中获取数据，需要一层一层的传下来，有的中间组件可能只是用来传递数据用，同理，Com4向App传递数据，也要通过回调函数一层层往上传，再这样的情况下，代码实在是比较臃肿，不仅写出了很多不必要的代码，而且使得数据也比较难以理解。

假设有这么一个中央仓库，任何组件都可以直接存储和更改数据，而不需要一层层的传递

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703204811.png"/>
</center>

不管组件处于多深的层次，都可以从store中直接拿取数据。所以这个想法怎么做到呢?

## Context

首先来介绍React中你可能永远不会用到的特性context。我们在父组件中它下面的子组件通过context的方式提供数据，其中的任意子组件均可以通过context访问数据，这就使得context相当于中央仓库，那怎么用呢(为了演示方便，就写父子两层)?

<iframe height="400" style="width: 100%;" scrolling="no" title="Context的使用" src="https://codepen.io/lastknightcoder/embed/JjdGQgL?height=675&theme-id=dark&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lastknightcoder/pen/JjdGQgL'>Context的使用</a> by LastKnightCoder
  (<a href='https://codepen.io/lastknightcoder'>@lastknightcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

在上面用到了PropTypes，所以需要下载prop-types，使用npm下载

```shell
cnpm install prop-types --save
```

首先父组件提供context要声明提供的数据名称以及对应的类型

```js
static childContextTypes = {
    data: PropTypes.string
}
```

然后在getChildContext中返回数据

```js
getChildContext () {
    return {
      data: "data"
    }
}
```

而子组件要使用父组件提供的数据，首先要声明要使用的数据是什么以及类型

```js
static contextTypes = {
    data: PropTypes.string
}
```

只有这样才可以通过this.context获取想要的数据

```jsx
<div>Child中获取Context：{this.context.data}</div>
```

context 打破了组件和组件之间通过 props 传递数据的规范，极大地增强了组件之间的耦合性。而且，就如全局变量一样，context 里面的数据能被随意接触就能被随意修改，每个组件都能够改 context 里面的内容会导致程序的运行不可预料。

但是这种机制对于前端应用状态管理来说是很有帮助的，因为毕竟很多状态都会在组件之间进行共享，context 会给我们带来很大的方便。一些第三方的前端应用状态管理的库（例如 Redux）就是充分地利用了这种机制给我们提供便利的状态管理服务。但我们一般不需要手动写 context，也不要用它，只需要用好这些第三方的应用状态管理库就行了。

## Redux + React Redux

本来还想继续写Redux，React-Redux的，但是我发现[React.js 小书](http://huziketang.mangojuice.top/books/react/)写的实在是太好了，推荐仔细阅读其中的Redux和React-Redux的部分。

# React Hooks

使用React Hooks，使得在函数式组件可以有状态，也可以仿照生命周期函数，甚至可以替代React Redux。所谓的Hooks，就是指一些函数，通过这些函数可以在函数式组建中实现类组件的功能。

钩子函数一般是useXxx的形式，所有的钩子函数都只能写在函数式组件中。

## useState

我们来看一下如何在函数式组件中使用state

<iframe height="265" style="width: 100%;" scrolling="no" title="useState的使用" src="https://codepen.io/lastknightcoder/embed/WNvrVOo?height=265&theme-id=dark&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lastknightcoder/pen/WNvrVOo'>useState的使用</a> by LastKnightCoder
  (<a href='https://codepen.io/lastknightcoder'>@lastknightcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

通过useState来创建一个状态，接收一个参数，这个参数就是状态的初始值，返回一个数组，数组包含两个元素，第一个元素就是状态，第二个元素是一个方法，该方法用来设置状态的，如上面

```jsx
const [number, setNumber] = useState(0)
```

其中number就是状态，初始值为0，而setNumber是用来设置number的(二者的名字可以随便起，什么changeNumber随意)，每当我们点击+1按钮，都会将当前的number加一。

## useEffect

useEffect主要是用来执行一些副作用的代码，比如拿数据

<iframe height="265" style="width: 100%;" scrolling="no" title="useEffect" src="https://codepen.io/lastknightcoder/embed/poJgMar?height=265&theme-id=dark&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lastknightcoder/pen/poJgMar'>useEffect</a> by LastKnightCoder
  (<a href='https://codepen.io/lastknightcoder'>@lastknightcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

useEffect接收两个参数，第一个参数是回调函数，第二个参数是一个数组，它会监视数组中的元素，当数组中的元素发生改变时，就会执行回调函数，所以当我们点击按钮改变data时，由于data发生改变，useEffect中的回调函数会被执行。如果希望是componentDidMount生命周期函数的特点，即只在组件挂载是执行一次，那么可以为第二个参数传入一个空数组。

## useContext

useContext的作用正如在React Redux中介绍的Context中一样，可以使得子组件无论在什么深度都可以直接访问父组件提供的Context，这个给个例子大家就可以明白

<iframe height="265" style="width: 100%;" scrolling="no" title="useContext的使用" src="https://codepen.io/lastknightcoder/embed/vYOLorB?height=265&theme-id=dark&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lastknightcoder/pen/vYOLorB'>useContext的使用</a> by LastKnightCoder
  (<a href='https://codepen.io/lastknightcoder'>@lastknightcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
## useReducer

如何仔细阅读过React.js小书，那么想必你已经理解了reducer的作用，useReducer接收一个reducer和初始状态initState，返回state和dispatch，如下

<iframe height="265" style="width: 100%;" scrolling="no" title="useReducer" src="https://codepen.io/lastknightcoder/embed/BaNjXOL?height=265&theme-id=dark&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lastknightcoder/pen/BaNjXOL'>useReducer</a> by LastKnightCoder
  (<a href='https://codepen.io/lastknightcoder'>@lastknightcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

## 结合useContext和useReducer

我们可以将useContext和useReducer结合，来达到React-Redux的效果，具体就是将useReducer产生的state和dispatch作为Context传下去，如下

<iframe height="959" style="width: 100%;" scrolling="no" title="useContext与useReducer结合" src="https://codepen.io/lastknightcoder/embed/BaNjXGQ?height=959&theme-id=dark&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/lastknightcoder/pen/BaNjXGQ'>useContext与useReducer结合</a> by LastKnightCoder
  (<a href='https://codepen.io/lastknightcoder'>@lastknightcoder</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

# 富文本编辑器

既然是搭建一个基于富文本编辑器的博客系统，那么就要用到富文本编辑器组件，这里我们选择[BraftEditor](https://braft.margox.cn/)。

使用npm安装到你的项目

```shell
cnpm install braft-editor --save
```

新建pages文件夹，并且在pages中新建文件夹RichText，在RichText中新建index.jsx，内容如下

```jsx
import React, {useState} from 'react'
import BraftEditor from 'braft-editor'
import 'braft-editor/dist/index.css'

function RichText(props) {

    const [editorState, setEditorState] = useState(BraftEditor.createEditorState("<p>123</p>"))

    const handleEditorChange = (editorState) => {
        
    }

    const handleEditorSave = (editorState) => {

    }

    return (
        <BraftEditor
            value={editorState}
            onChange={handleEditorChange}
            onSave={handleEditorSave}
        />
    )
}

export default RichText
```

我们在src下的index.js中引入该组件，并渲染到dom中

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import RichText from './pages/RichText';

ReactDOM.render(<RichText />, document.getElementById("root"));
```

这时你使用yarn start或者npm start启动项目，在端口3000看到的页面应该是这样的

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200214145449.png"/>
</center>
现在我们来看BraftEditor组件接收的三个props，分别是

- value
- onChange
- onSave

我们一个个来看。

首先看value，它是BraftEditor组件要显示的内容，它的值是一个editorState对象，我们可以通过BraftEditor.createEditorState()创建editorState对象，它接收html字符串或者一个raw对象(raw对象是它自定义的对象，一般我们保存富文本的内容就是保存raw对象)，也可以通过editorState实例转化为html字符串或者raw对象，如

```jsx
// 创建edirorState对象
const editorState = BraftEditor.createEditor("<p>123</p>");
const htmlContent = editorState.toHTML();
const rawContent = editorState.toRAW();
```

再者来看onChange，它是当BraftEditor发生改变时调用的回调函数，它会传入一个editorState对象，这个editorState对象代表的就是当前的文本内容。

最后看onSave，它是当按下Ctrl + S保存时会调用的方法，也会将代表当前文本内容的editorState传入。

我们来做一个简单的案例，当按下Ctrl + S时，将数据以raw对象的格式保存在localStorage中，当富文本加载时，从localStorage中读取数据

```jsx
import React, {useState, useEffect} from 'react'
import BraftEditor from 'braft-editor'
import 'braft-editor/dist/index.css'

function RichText(props) {

    const [editorState, setEditorState] = useState(BraftEditor.createEditorState(null))

    useEffect(() => {
        const rawContent = localStorage.getItem("rawContent") || null;
        setEditorState(BraftEditor.createEditorState(rawContent))
    }, [])

    const handleEditorChange = (editorState) => {
        
    }

    const handleEditorSave = (editorState) => {
        const rawContent = editorState.toRAW();
        localStorage.setItem("rawContent", rawContent)
    }

    return (
        <BraftEditor
            value={editorState}
            onChange={handleEditorChange}
            onSave={handleEditorSave}
        />
    )
}

export default RichText
```

这里就增加了两个地方，第一个是增加了useEffect，作用是当组件挂载后从localStorage中读取数据(既然从localStorage初始化，那么原来的"\<p\>123\</p\>"就换为了null)，第二个是更改了handleEditorSave，当保存时将数据保存在localStorage中，上面的代码都很容易理解，不再解释。如果成功了的话，在富文本编辑器中输入并按Ctrl + S保存，然后刷新，数据会原样的重现。

# 页面布局

我们来看一下项目的每个页面的布局

Home

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200214153650.png"/>
</center>
Edit

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200214153119.png"/>
</center>
Display

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200214153418.png"/>
</center>
Login

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200214153540.png"/>
</center>
其中Home, Display, Edit都是下面这样的布局

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200214154437.png" width="60%"/>
</center>


而Login的布局是

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200214154823.png" width="60%" />
</center>


所以我们抽离出布局组件，Home，Edit,Display三者的布局抽离为BasicLayout，而Login的布局抽离为LoginLayout组件，注意到他们的Heder都是相同的，所以我们把Header抽离为一个公共的组件。

## Header

在src下新建components目录，在其中新建Header文件夹，在其中新建index.jsx和index.module.css。index.jsx的内容如下

```jsx
import React from 'react'
import styles from './index.module.css'

function Header(props) {

    const toEdit = (e) => {
        e.preventDefault()
    }
    
    const toHome = (e) => {
        e.preventDefault()
    }

    return (
        <div className={styles.header}>
            <div className={styles.nav}>
                <ul>
                    <li><a href="/home" onClick={toHome}>首页</a></li>
                    <li><a href="/edit" onClick={toEdit}>写博客</a></li>
                </ul>
            </div>
            <div className={styles.desc}>
                Coder
            </div>
        </div>
    )
}

export default Header
```

index.module.css中的内容为

```css
.nav {
    width: 100%;
    height: 100px;
    position: relative;
}

.nav ul {
    position: absolute;
    top: 0;
    right: 0;
    padding: 0;
    margin: 0;
}

.nav ul li {
    list-style: none;
    float: left;
    width: 80px;
    height: 30px;
    line-height: 30px;
    text-align: center;
    padding-top: 5px;
}

.nav ul li a {
    text-decoration: none;
    color: white;
    font-family: Consolas, "楷体";
}

.nav ul li:hover {
    border-bottom: 1px solid #FFF;
    box-shadow: 0 0 10px #FFF inset;
}

.desc {
    width: 100%;
    height: 100px;
    font-size: 40px;
    line-height: 100px;
    color: white;
    font-family: Consolas, "楷体";
}
```

上面的内容想必还是很容易理解的，其中Header中的两个a标签的点击事件均没有处理，这两个a标签是用于做路由跳转用的，等用到在回过头来补充。

## BasicLayout

在src下新建layouts文件夹，在其中新建文件夹BasicLayout，在BasicLayout中新建index.jsx和index.module.css，其中index.jsx的内容为

```jsx
import React from 'react'
import styles from './index.module.css'
import Header from './../../components/Header'

function BasicLayout(props) {
    const { children } = props;

    return (
        <div className={styles.box}>
            <div className={styles.container}>
                <Header />
                <div className={styles.main}>
                    <div className={styles.content}>
                        {children}
                    </div>
                    <div className={styles.aside}>
                    </div>
                </div>
            </div>
        </div>
    )
}

export default BasicLayout
```

其中index.module.css中的内容为

```css
.box {
    width: 100%;
    background: #d8e2eb url(./../../assets/img/bg.jpg) no-repeat top center;
    min-height: 100vh;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 50px;
}

.main {
    display: flex;
}

.content {
    flex: 3;
    margin-right: 20px;
}

.aside {
    flex: 1;
}
```

这里要用到背景图片，我们在src下新建assets文件夹，然后新建img，在其中放入背景图片，背景图片在这里

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200214163456bg.jpg"/>
</center>
接在src下新建common.css，以消除内外边距，设置字体

```css
* {
    margin: 0;
    padding: 0;
}

div {
    font-family: Consolas, "楷体";
    box-sizing: border-box;
}
```

现在我们在src/index.js中引用该布局看看效果

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

import RichText from './pages/RichText';
import BasicLayout from './layouts/BasicLayout'

import './common.css'

ReactDOM.render(<BasicLayout>
                  <RichText />
                </BasicLayout>, 
                document.getElementById("root"));

```

如下：

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200214171622.png"/>
</center>

## LoginLayout

在layouts下新建LoginLayout文件夹，在LoginLayout中新建index.jsx和index.module.css。有了BasicLayout的经验，代码的内容不必解释，直接上代码。index.jsx

```jsx
import React from 'react'
import styles from './index.module.css'
import Header from './../../components/Header'

function LoginLayout(props) {
    const { children } = props;

    return (
        <div className={styles.box}>
            <div className={styles.container}>
                <Header />
                <div className={styles.main}>
                    {children}
                </div>
            </div>
        </div>
    )
}

export default LoginLayout
```

index.module.css

```css
.box {
    width: 100%;
    background: #d8e2eb url(./../../assets/img/bg.jpg) no-repeat top center;
    min-height: 100vh;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 50px;
}

.main {
    width : 300px;
    margin: 0 auto;
}
```

# 页面路由

为了使用路由，先下好react-router-dom

```shell
cnpm install react-router-dom --save
```

该blog项目总共有四个页面

- Home：页面主页
- Display：阅读文章的页面
- Edit：编辑文章的页面
- Login：登录页面

为了演示路由，在pages中分别新建Home, Display, Edit, Login四个文件夹，并在每个文件夹中新建index.jsx，同时在Edit文件夹了新建components文件夹，将RichText文件夹全部移入到这个文件夹中(因为RichText富文本编辑器属于编辑文章页面的一部分)，简单的在每个文件的index.jsx写下一些内容，具体的内容在后面添加。

```jsx
// Home/index.jsx
import React from 'react'
import BasicLayout from './../../layouts/BasicLayout'
import {withRouter} from 'react-router-dom'

function Home(props) {
    return (
        <BasicLayout>
            Home
        </BasicLayout>
    )
}

export default withRouter(Home)
```

```jsx
// Display/index.jsx
import React from 'react'
import BasicLayout from './../../layouts/BasicLayout'
import {withRouter} from 'react-router-dom'

function Display(props) {
    return (
        <BasicLayout>
            Display
        </BasicLayout>
    )
}

export default withRouter(Display)
```

```jsx
// Edit/index.jsx
import React from 'react'
import BasicLayout from './../../layouts/BasicLayout'
import {withRouter} from 'react-router-dom'
import RichText from './components/RichText'

function Edit() {
    return (
        <BasicLayout>
            <RichText />
        </BasicLayout>
    )
}

export default withRouter(Edit)
```

```jsx
// Login/index.jsx
import React from 'react'
import {withRouter} from 'react-router-dom'
import LoginLayout from './../../layouts/LoginLayout'

function Login(props) {
    return (
        <LoginLayout>
            login
        </LoginLayout>
    )
}

export default withRouter(Login)
```

设计页面路由如下

|path | component | redirect |
|:--- |:--- |:--- |
|/home | Home | |
|/display/:id | Display| |
|/edit| Edit| |
|/edit:id| Edit| |
|/login| Login| |
| / | | /home |

注意到Edit组件对应两个路径，因为编辑文章有两种情况，第一种是添加文章，这时是/edit路径，第二种是编辑文章，这时需要传入文章的id，所以这时是/edit/:id路径。

在src下新建文件夹config，在config新建routes.js，里面设置路由信息，如下

```js
import Home from './../pages/Home/index'
import Display from './../pages/Display/index'
import Edit from './../pages/Edit/index'
import Login from './../pages/Login/index'

export default [
    {
        path: "/login",
        component: Login
    },
    {
        path: "/home",
        component: Home
    },
    {
        path: "/display/:id",
        component: Display
    },
    {
        path: "/edit",
        component: Edit
    },
    {
        path: "/edit/:id",
        component: Edit
    },
    {
        path: "/",
        redirect: "/home",
        exact: true
    },
]
```

在src下新建router.js，用来渲染路由，内容如下

```jsx
import React from 'react'
import routes from './config/routes'
import {Switch, BrowserRouter as Router, Route, Redirect } from 'react-router-dom'
import { createBrowserHistory } from 'history';

const history = createBrowserHistory();

// 返回可能是Redirect 也可能是Route 所以使用RouteItem封装
const RouteItem = (props) => {
    const { path, component: Component, redirect, key, exact } = props;

    if (redirect) {
        return <Redirect from={path} to={redirect} key={key} />
    }

    return (
        <Route
            key={key}
            exact={exact}
            path={path}
            render={componentProps => {
                return (
                    <Component {...componentProps} />
                )
            }}
        />
    );
};

const router = () => {
    return (
        <Router>
            {/* Switch 唯一匹配  */}
            <Switch>
                {routes.map((item, id) => {
                    return RouteItem({ key: id, ...item })
                })}
            </Switch>
        </Router>
    );
};


export default router;
```

接着在src/index.js中渲染出来

```jsx
import ReactDOM from 'react-dom';

import router from './router'

import './common.css';

ReactDOM.render(router(), document.getElementById("root"));
```

接着启动项目(npm start或yarn start)，改变浏览的url(如localhost:3000/display)看看页面是否能成功跳转(我这里是没有问题的，如果你不能的话，回过头仔细看看吧)。

# 页面编写

## antd表单的使用

要使用antd，先下载antd

```shell
cnpm install antd --save
```

首先看一个表单的例子，在src下新建一个test文件夹，新建TextAntdForm.jsx，内容如下(先不管看得懂看不懂，后面解释)

```jsx
import React from 'react'
import {Form, Input, Button} from 'antd'

function TextAntdForm(props) {

    const {getFieldDecorator, validateFields} = props.form

    const handleSubmit = (event) => {
        event.preventDefault();
        validateFields((error, values) => {
            if (!error) {
                console.log(values);
            }
        })
    }

    return(
        <div style={{width: "300px", margin: "100px auto", fontFamily: "Consolas, '楷体'"}}>
            <Form onSubmit={handleSubmit}>
                <Form.Item label="用户名">
                    {getFieldDecorator('title', {
                      rules: [{
                        required: true,
                        message: '请输入用户名',
                      }],
                    })(
                      <Input size="large" placeholder="请输入用户名"/>
                    )}
                </Form.Item>
                <Form.Item label="密码">
                    {getFieldDecorator('password', {
                      rules: [{
                        required: true,
                        message: '请输入密码',
                      }],
                    })(
                      <Input.Password size="large" placeholder="请输入密码"/>
                    )}
                </Form.Item>

                <Form.Item>
                    <Button size="large" type="primary" htmlType="submit">提交</Button>
                </Form.Item>
            </Form>
        </div>
    )
}

export default Form.create()(TextAntdForm)
```

修改src/index.js渲染该组件(记得引入antd/dits/antd.css，否则antd组件没有样式)

```jsx
import React from 'react'
import ReactDOM from 'react-dom';

// import router from './router'
import TestAntdForm from './test/TestAntdForm'
import 'antd/dist/antd.css'
import './common.css';

// ReactDOM.render(router(), document.getElementById("root"));
ReactDOM.render(<TestAntdForm />, document.getElementById("root"));
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703211521.png" width="80%"/>
</center>

现在来解释上面的代码，首先看最后一行

```jsx
Form.create()(TextAntdForm)
```

还记得高阶组件吗，Form.create()就是一个高阶组件，他会向组件的props中注入form，form提供了一些API，这里使用了两个：

- getFieldDecorator：用于和表单进行双向绑定

```jsx
getFieldDecorator('title', {
  rules: [{
    required: true,
    message: '请输入用户名',
  }],
})(
  <Input size="large" placeholder="请输入用户名"/>
)
```

上面将Input与表单项进行了绑定，getFieldDecorator接收两个参数，第一个参数是id，根据它可以获取输入控件的值或者设置输入控件的值，是必填项；第二个参数是options，里面可以有很多属性，这里使用了rules，定义了校验的规则，required表示是否必填，message表示未填时显示的消息文字。

- validateFields：校验

里面接受一个回调函数，回调函数接收两个参数，第一个参数为error，当不满足校验规则时error的值非空，第二个参数是values，会将绑定表单的值以对象的形式传给values，键就是在getFieldDecorator传入的id。

上面在提交表单后，会调用Form的onSubmit回调函数，在回调函数，我们对数据进行了校验，如果没有问题的话，我们可以将输入表单的键值对以对象的形式获取到values，并打印出来

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703211633.png"/>
</center>

## 页面数据

我们将数据设置为一个数组datas，它的格式如下

```jsx
datas = [
  {title: , brief: , isTop: , content: }
  {title: , brief: , isTop: , content: }
]
```

Home组件根据datas展示数据，Edit和Display组件根据id和datas获取要展示的数据，由于多个组件都要用到数据，所以这里使用useContext和useReducer来分发数据。

在src下新建Provider.jsx，用来提供state和dispatch，内容如下

```jsx
import React, {useReducer} from 'react'

export const Context = React.createContext();

const reducer = (state, action) => {
    const tempDatas = state.datas
    // 按道理case后面跟的都是常量，我这里为了简单
    switch(action.type) {
        case 'insertData':
            tempDatas[tempDatas.length] = action.data;
            return {...state, datas: tempDatas}
        case 'updateData':
            tempDatas[action.id] = action.data
            return {...state, datas: tempDatas}
        case 'deleteData':
            tempDatas.splice(action.id, 1)
            return {...state, datas: tempDatas}
        case 'changeOperation':
            return {...state, operation: action.operation}
        default:
            return state
    }
}

const initState = {
    // 随便写的数据用来实验 随后会删掉
    datas: [{title: 'aaa', isTop: true, brief: 'hahah'}, content: '<p>123</p>'],
    // 用来判断是添加文章还是编辑文章
    operation: 'ADD'
}

function Provider(props) {
    const [state, dispatch] = useReducer(reducer, initState)
    const {children} = props
    
    return (
        <Context.Provider value={{state, dispatch}}>
            {children}
        </Context.Provider>
    )
}

export default Provider
```

更改router.js，在最上面加上Provider

```jsx
import Provider from './Provider'
// 上面没有变化，除了import Provider，故此省略
const router = () => {
    return (
        <Provider>
            <Router history={history}>
                <Switch>
                    {routes.map((item, id) => {
                        return RouteItem({ key: id, ...item, history: history })
                    })}
                </Switch>
            </Router>
        </Provider>
    );
};

export default router
```

## Home

观察Home页面

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200214212726.png"/>
</center>
发现Home是由这一个个Item组成，Item中的数据正是datas数组中每一个元素的内容，在Home中新建文件夹Item，并在Item中新建index.jsx和index.module.css。index.jsx:

```jsx
import React from 'react'
import styles from './index.module.css'
import {Modal} from 'antd'

const {confirm}  = Modal

function Item(props) {

    const { 
        data, 
        login, 
        handleDelete, 
        index, 
        handleToEdit, 
        handleToDisplay 
    } = props;

    const toDisplay = (event) => {
        event.preventDefault();
        handleToDisplay(index)
    }

    const toEdit = (event) => {
        event.preventDefault();
        handleToEdit(index)
    }

    const toDelete = (event) => {
        event.preventDefault();
        confirm({
            title: `你确定要删除${data.title}`,
            content: '删除后内容不可恢复',
            onOk() {
                handleDelete(index)
            },
            onCancel() {

            },
        }); 
    }

    return (
        <div className={styles.item}>
            <h2><a href={`/display/${data.id}`} onClick={toDisplay}>{data.title}</a></h2>
            <hr />
            <div className={styles.abstract}>
                {data.brief}
            </div>
            <div className={styles.readmore}>
                <a href={`/display/${data.id}`} onClick={toDisplay}>阅读更多</a>
            </div>
            {/* 当登录失显示编辑本文和删除本文 */}
            {login ? 
            <div className={styles.edit}>
                <a href={`/edit/${data.id}`} onClick={toEdit}>编辑本文</a>
            </div> : ""} 
            {login ? 
                <div className={styles.delete}>
                    <a href="/delete" onClick={toDelete}>删除本文</a>
                </div>: ""}
            {/* 当isTop为1时显示置顶图标 */}
            {data.isTop ? 
            <div className={styles.isTop}>
                <svg viewBox="0 0 1024 1024">
                    <path d="M0 0h1024v1024z" fill="#7ED321"></path>
                    <path d="M571.733333 157.866667l17.066667-12.8-83.2-83.2L552.533333 14.933333l183.466667 183.466667-46.933333 46.933333-81.066667-81.066666-17.066667 12.8 100.266667 100.266666-14.933333 14.933334-102.4-102.4c-6.4 4.266667-10.666667 8.533333-17.066667 10.666666l72.533333 72.533334-110.933333 110.933333 36.266667 36.266667-14.933334 14.933333L313.6 209.066667l14.933333-14.933334 36.266667 36.266667 110.933333-110.933333 61.866667 61.866666c6.4-4.266667 10.666667-8.533333 17.066667-10.666666l-96-96 14.933333-14.933334 98.133333 98.133334z m-72.533333 209.066666l17.066667-17.066666-117.333334-117.333334-17.066666 17.066667 117.333333 117.333333z m27.733333-29.866666l14.933334-14.933334L426.666667 204.8l-14.933334 14.933333 115.2 117.333334z m27.733334-27.733334l17.066666-14.933333-117.333333-117.333333-17.066667 14.933333 117.333334 117.333333z m27.733333-25.6l14.933333-14.933333L482.133333 149.333333l-14.933333 14.933334 115.2 119.466666z m10.666667-202.666666L554.666667 44.8l-21.333334 21.333333 38.4 38.4 21.333334-23.466666z m57.6 57.6l-40.533334-40.533334-21.333333 21.333334 40.533333 40.533333 21.333334-21.333333zM704 192l-38.4-38.4-21.333333 21.333333L682.666667 213.333333l21.333333-21.333333zM571.733333 471.466667l12.8-21.333334c8.533333 10.666667 17.066667 19.2 25.6 27.733334 6.4 6.4 12.8 6.4 21.333334-2.133334l172.8-172.8-38.4-38.4 17.066666-17.066666 87.466667 87.466666-17.066667 17.066667-29.866666-29.866667-177.066667 177.066667c-14.933333 14.933333-29.866667 14.933333-44.8 0l-29.866667-27.733333z m302.933334 21.333333l-44.8 44.8c-27.733333 25.6-55.466667 40.533333-83.2 44.8-27.733333 2.133333-59.733333-6.4-96-25.6l6.4-25.6c34.133333 19.2 64 27.733333 87.466666 25.6 23.466667-4.266667 46.933333-14.933333 68.266667-36.266667l44.8-44.8 17.066667 17.066667z m132.266666-21.333333l-17.066666 19.2-55.466667-55.466667c-10.666667 8.533333-19.2 17.066667-29.866667 23.466667l51.2 51.2-119.466666 119.466666-17.066667-17.066666 102.4-102.4-76.8-76.8-104.533333 100.266666-17.066667-17.066666 121.6-121.6 42.666667 42.666666c10.666667-6.4 19.2-14.933333 29.866666-23.466666L861.866667 362.666667l17.066666-17.066667 128 125.866667zM802.133333 682.666667h-25.6c2.133333-25.6 2.133333-55.466667-2.133333-89.6h23.466667c4.266667 34.133333 4.266667 64 4.266666 89.6z" fill="#FFFFFF"></path>
                </svg>
            </div> : ""}
        </div>
    )
}

export default Item
```

注意到Item为展示组件，只负责数据的展示，而不负责数据的处理、获取，Item的数据、数据的操作都是从props中获取的，这些操作都由Item的容器组件Home来完成。

我们会将login保存在sessionStorage，login是一个布尔值，保存了是否登录的信息，true表示登录，false表示未登录，根据是否登录，决定是否将删除和编辑的操作暴露出来。同时我们也将根据data的isTop是否为true来显示是否置顶的svg图样(该图样来自CSDN的置顶图样)。

index.module.css

```css
.item {
    width: 100%;
    height: 150px;
    background-color: white;
    margin-bottom: 40px;
    border-radius: 4px;
    padding-left: 25px;
    padding-right: 20px;
    padding-top: 20px;
    position: relative;
    box-shadow:0px 0px 6px 6px #FFF;
}

.item a {
    text-decoration: none;
    color: #40759b;
}

.item a:hover {
    text-decoration: underline;
}

.abstract {
    width: 100%;
    padding-top: 30px;
}

.readmore {
    position: absolute;
    font-size: 14px;
    bottom: 10px;
    right: 10px;
}

.edit {
    position: absolute;
    font-size: 14px;
    bottom: 10px;
    right: 75px;
}

.delete {
    position: absolute;
    font-size: 14px;
    bottom: 10px;
    right: 140px;
}

.isTop {
    position: absolute;
    width: 50px;
    top: 0;
    right: 0;
}
```

现在进入Home组件，修改Home/index.jsx如下

```jsx
import React, { useContext } from 'react'
import BasicLayout from './../../layouts/BasicLayout'
import Item from './components/Item'
import {withRouter} from 'react-router-dom'
import {Context} from './../../router'
import { message } from 'antd'

function Home(props) {

    const {history} = props
    // 使用useContext获取Provider提供的数据
    const {state, dispatch} = useContext(Context);
    const login = sessionStorage.getItem("login");
    const datas = state.datas;

    const handleDelete = (id) => {
        dispatch({type: "deleteData", id});
        message.success('删除成功');
    }

    const handleToEdit = (id) => {
        dispatch({type: "changeOperation", operation: "EDIT"})      
        history.push(`/edit/${id}`);
    }

    const handleToDisplay = (id) => {
        history.push(`display/${id}`);
    }

    return (
        <div>
            <BasicLayout>
                {datas.map((data, index) => {
                    return <Item 
                        index={index} 
                        data={data} 
                        key={index} 
                        login={login}
                        handleDelete={handleDelete}
                        handleToEdit={handleToEdit}
                        handleToDisplay={handleToDisplay}
                        />
                })}
            </BasicLayout>
            
        </div>
    )
}

export default withRouter(Home)
```

现在启动项目(npm start)，观察到页面如下

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200214224320.png"/>
</center>
说明Home页面已经成功了(由于在sessionStorage中没有login，所以删除本文和编辑本文均显示不出来，当点击阅读更多时，会跳转到Display的页面)。

## Display

我们来观察Display的页面

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200214225458.png"/>
</center>
发现Display页面有一个背景为白色的内容区和一个按钮，这个按钮根据是否有登录来决定是否暴露出来，所以我们在Display中新建一个components文件夹，在里面新建一个Content文件夹，在Content文件夹中新建index.jsx和index.module.css。首先Content是一个展示组件，所以它的数据全部都由Display提供，所有的数据操作也由Display传入回调函数进行处理。

index.jsx如下

```jsx
import React from 'react'
import { Button } from 'antd'
import styles from './index.module.css'

function Content(props) {
    const {login, htmlContent, handleToEdit} = props

    const toEdit = () => {
        handleToEdit();
    }

    return(
        <div className={styles.display}>
            <div className="braft-output-content"  style={{minHeight: "425px", backgroundColor: "#FFF", padding: "50px 25px", fontSize: "16px", maxWidth: "850px"}} dangerouslySetInnerHTML={{__html: htmlContent}} >

            </div>

            {login && 
                <div className={styles.edit}>
                    <Button type="primary" onClick={toEdit}>编辑文章</Button>
                </div>
            }
        </div>
    )
}

export default Content
```

想必上面的代码还是比较容易理解的，index.module.css的内容如下

```css
.display {
    position: relative;
}

.display ul, .display ol {
    padding-left: 30px;
}

.edit {
    position: absolute;
    top: 10px;
    right: 10px;
}
```

所以Display中的内容如下

```jsx
import React, {useContext} from 'react'
import BasicLayout from './../../layouts/BasicLayout'
import {withRouter} from 'react-router-dom'
import Content from './components/Content'
import {Context} from './../../Provider'
import BraftEditor from 'braft-editor'

function Display(props) {
    const {history} = props
    // 获取传过来的id
    const index = Number(history.location.pathname.split("/")[2])
    const {state, dispatch} = useContext(Context)
    const htmlContent = BraftEditor.createEditorState(state.datas[index].content).toHTML()
    const login = sessionStorage.getItem("login")

    const handleToEdit = () => {
        dispatch({type: "changeOperation", operation: "EDIT"});
        history.push(`/edit/${index}`);
    }

    return (
        <BasicLayout>
            <Content
                htmlContent={htmlContent}
                login = {login}
                handleToEdit = {handleToEdit}
            />
        </BasicLayout>
    )
}

export default withRouter(Display)
```

至此Display页面设计完毕。

## Edit

在写Edit页面之前，来改造一下RichText组件，我们要将RichText做成展示组件，所有的数据都由Edit提供，所有的数据处理也由Edit处理，修改如下

```jsx
import React from 'react'
import BraftEditor from 'braft-editor'
import 'braft-editor/dist/index.css'

function RichText(props) {

    const {value, onChange} = props

    const handleEditorChange = (editorState) => {
        onChange(editorState)
    }

    return (
        <BraftEditor
            value={value}
            onChange={handleEditorChange}
        />
    )
}

export default RichText
```

现在我们来看一下Edit页面的结构

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200215000232.png"/>
</center>
我们使用antd的表单来做成这件事情，在前面已经介绍过antd表单的使用，所以这里不多加介绍，直接上代码

```jsx
import React, {useEffect, useContext} from 'react'
import RichText from './components/RichText'
import { withRouter } from 'react-router-dom'
import { Form, Input, Button, message, Checkbox } from 'antd'
import BasicLayout from './../../layouts/BasicLayout'
import BraftEditor from 'braft-editor'
import {Context} from './../../Provider'


function Edit(props) {

  const {history} = props
  const FormItem = Form.Item;
  const { getFieldDecorator, validateFieldsAndScroll } = props.form;
  const {state, dispatch} = useContext(Context)

  useEffect(() => {
    // 当组件加载后 如果是编辑文章 根据id获取数据 然后显示 
    // 如果是添加操作，则不加载数据 直接显示空白内容
    if(state.operation === 'EDIT') {
        const index = Number(history.location.pathname.split("/")[2]) 
        const data = state.datas[index]
        // setFieldsValue为表单设置内容
        props.form.setFieldsValue({
          ...data, 
          content: BraftEditor.createEditorState(data.content)
        })
    }
  }, [])

  const handleSubmit = (event) => {
    event.preventDefault();
    
    validateFieldsAndScroll((err, values) => {
      if(!err) {
        // 如果是通过添加按钮进来的 那么拿到数据保存 然后跳转到home页面
        if(state.operation === 'ADD') {
            dispatch({type: 'insertData', data: {
                ...values,
                content: values.content.toRAW()
            }});
            message.success("添加成功");
            history.push("/home");
            //如果是编辑文章进来的，更新数据 然后跳转到home
        } else if (state.operation === "EDIT") {
            const id = Number(history.location.pathname.split("/")[2]);
            dispatch({type: "updateData", id, data: {
                ...values, 
                content: values.content.toRAW(),
            }});
            message.success("更新成功");
            history.push("/home");
        }
      }
    })
  }

  
  return (
      <BasicLayout>
        <div>
          <Form onSubmit={handleSubmit}>
            <FormItem labelAlign="left" label="文章标题">
                {getFieldDecorator('title', {
                  rules: [{
                    required: true,
                    message: '请输入标题',
                  }],
                })(
                  <Input size="large" placeholder="请输入标题"/>
                )}
              </FormItem>
            <FormItem size="large" label="文章摘要">
                {getFieldDecorator('brief', {
                  rules: [{
                    required: true,
                    message: '请输入摘要',
                  }],
                })(
                  <Input.TextArea style={{fontSize: "16px"}} placeholder="请输入摘要"/>
                )}
              </FormItem>
            <FormItem>
                {getFieldDecorator('isTop', {
                  valuePropName: 'checked',
                })(
                  <Checkbox>
                    是否置顶
                  </Checkbox>,
                )}
              </FormItem>
            <FormItem label="文章正文">
                {getFieldDecorator('content', {
                  validateTrigger: 'onBlur',
                  rules: [{
                    required: true,
                    message: "请输入正文"
                  }],
                })(
                  <RichText />
                )}
              </FormItem>
            <FormItem>
                <Button size="large" type="primary" htmlType="submit">提交</Button>
              </FormItem>
          </Form>
        </div>
      </BasicLayout>
  )
}

// 为Edit注入form
export default withRouter(Form.create()(Edit))
```

上面的代码虽然有点长，但是都是比较容易理解的。注意，虽然我们没有为RichText传入value和onChange，但是由于RichText和表单项进行了双向绑定，所以表单会注入value和onChange。

## Login

Login页面应该是最简单的，只要用我们在前面antd表单示例里面的表单就可以完成，所以直接上代码如下

```jsx
import React from 'react'
import {withRouter} from 'react-router-dom'
import {Form, Button, Input, message } from 'antd'
import LoginLayout from './../../layouts/LoginLayout'

function Login(props) {
    const { form, history } = props;

    const FormItem = Form.Item;

    const {getFieldDecorator, validateFields} = form

    const handleSubmit = (event) => {
        event.preventDefault();
        validateFields((err, values) => {
            if (!err) {
                if (values.adminId === "123" && values.password === "123") {
                    sessionStorage.setItem("login", true);
                    message.success("登录成功");
                    history.push("/home")
                } else {
                    message.error("用户名或密码错误")
                }
            }
        })
    }

    return (
        <LoginLayout>
          <Form onSubmit={handleSubmit}>
            <FormItem labelAlign="left" label="用户名">
                {getFieldDecorator('adminId', {
                  rules: [{
                    required: true,
                    message: '请输入用户名',
                  }],
                })(
                  <Input size="large" placeholder="请输入用户名"/>
                )}
            </FormItem>
            <FormItem size="large" label="密码">
                {getFieldDecorator('password', {
                  rules: [{
                    required: true,
                    message: '请输入密码',
                  }],
                })(
                  <Input.Password size="large" placeholder="请输入密码"/>
                )}
            </FormItem>
            
            <FormItem>
                <Button size="large" type="primary" htmlType="submit">提交</Button>
            </FormItem>
          </Form>
        </LoginLayout>
    )
}

export default withRouter(Form.create()(Login))
```

## 收尾

在这里还有一个小地方没有处理，那就是Header，里面的a标签的点击事件没有处理，并且我们希望在登录的情况下显示"写博客"，以及在登录的情况下显示"退出登录"，在未登录的情况下显示"登录"，所以修改Header如下(由于要用到history，所以要在Layout里给Header传入history，但是Layout也没有history，所以要在Home, Edit, Display, Login中给用到的Layout传入history，这里的代码就不贴出了，想必这样的事情对现在的你应该已经很简单了)

```jsx
import React, {useContext} from 'react'
import styles from './index.module.css'
import {Context} from './../../Provider'

function Header(props) {

    const {history} = props;
    const {dispatch} = useContext(Context);

    const login = sessionStorage.getItem("login");

    const toEdit = (e) => {
        e.preventDefault();
        dispatch({type: "changeOperation", operation: "ADD"});
        history.push("/edit");
    }
    
    const toHome = (e) => {
        e.preventDefault()
        history.push("/home")
    }

    const logout = (e) => {
        e.preventDefault()
        sessionStorage.removeItem("login");
        history.push("/login");
    }

    const login_ = (e) => {
        e.preventDefault()
        history.push("/login");
    }

    return (
        <div className={styles.header}>
            <div className={styles.nav}>
                <ul>
                    <li><a href="/home" onClick={toHome}>首页</a></li>
                    {login && <li><a href="/edit" onClick={toEdit}>写博客</a></li>}
                    {login ? <li><a href="/login" onClick={logout}>退出登录</a></li> : <li><a href="/login" onClick={login_}>登录</a></li>}
                </ul>
            </div>
            <div className={styles.desc}>
                Coder
            </div>
        </div>
    )
}

export default Header
```

接下来就是数据的持久化，我们希望将数据能够保存到localStorage，这样当页面刷新，关闭页面、浏览器,关机数据都能够保存。修改Provider.jsx

```jsx
import React, {useReducer} from 'react'

export const Context = React.createContext();

const saveData = (datas) => {
    localStorage.setItem("datas",JSON.stringify(datas) || [])
}

const loadData = () => {
    // 如果datas没有内容，则为空数组，因为后面用到datas.map，放止报错
    return JSON.parse(localStorage.getItem("datas")) || []
}

const reducer = (state, action) => {
    const tempDatas = state.datas
    // 每次对数据进行操作后都保存数据
    switch(action.type) {
        case 'insertData':
            tempDatas[tempDatas.length] = action.data;
            saveData(tempDatas)
            return {...state, datas: tempDatas}
        case 'updateData':
            tempDatas[action.id] = action.data
            saveData(tempDatas)
            return {...state, datas: tempDatas}
        case 'deleteData':
            tempDatas.splice(action.id, 1)
            saveData(tempDatas)
            return {...state, datas: tempDatas}
        case 'changeOperation':
            return {...state, operation: action.operation}
        default:
            return state
    }
}

const initState = {
    // 初始化从localStorage中读取数据
    datas: loadData(),
    operation: 'ADD'
}

function Provider(props) {
    const [state, dispatch] = useReducer(reducer, initState)
    const {children} = props
    
    return (
        <Context.Provider value={{state, dispatch}}>
            {children}
        </Context.Provider>
    )
}

export default Provider
```

# 登录权限控制

如果用户没有登录的话，是没有权利访问某些页面的，比如编辑页面，它不能够添加文章，也不能够编辑文章(虽然我们在未登录的情况下没有暴露这样的途径，如Display页面未登录没有编辑文章的按钮，Header未登录没有写文章的连接，但是可以通过url直接访问)，所以我们要做一些权限控制。

所以在访问路由时，我们要做权限的检查，修改router.js中的RouteItem方法，不能直接的返回Route，而是要给Route加一层验证，以决定是否返回，如下

```jsx
import Auth from './components/Auth'
// ...
const RouteItem = (props) => {
    const { path, component: Component, redirect, key, exact } = props;

    if (redirect) {
        return <Redirect from={path} to={redirect} key={key} />
    }

    return (
        <Route
            key={key}
            exact={exact}
            path={path}
            render={componentProps => {
                return (
                    // 对Component加以验证
                    <Auth history={history}>
                        <Component {...componentProps} />
                    </Auth>
                )
            }}
        />
    );
};
// ...
```

Auth组件正是用来做权限控制的，在src/components下新建Auth文件夹，并在其中新建index.jsx，如下

```jsx
function Auth(props) {
    const {children, history} = props

    const login = sessionStorage.getItem("login")

    // Edit页面不能直接访问 需要登录
    if (children.type.WrappedComponent.name !== "Edit") {
        return children
    } else {
        if (login) {
            return children
        }

        history.replace("/login")
        return null
    }

}

export default Auth
```

验证的逻辑也是十分的简单，如果不是Edit组件，则可以访问直接返回，如果是，则进一步判断是否登录，如果登录，则可以访问，否则来到登录页面。

至此，整个项目的工作已经大致完成了，可能有的地方还需要美化，比如富文本编辑器的代码美化，或者将数据保存在后台服务器等等。这一路走来可能你会感到有点不轻松，那么恭喜你，你获得了进步，如果你十分的轻松，那么这个项目对你来说还是有点容易。不过不管怎么样，希望你能够完全靠自己做一遍，也许你跟着我一路走来十分的顺利，这是因为一些坑我给你跳过去了，说实话，在我第一次做时，遇到到许许多多的坑，有很多不明白的点，有的地方明明十分的简单，可是我能卡几个小时甚至一两天，虽然辛苦，但是收获十分的巨大。所以我希望你能够独立的完成，去遇到一些坑，然后去找解决办法，在这个过程你会收获巨大，索性你已经完成了这个项目，所以对于这个项目要做出什么样的效果以及功能已经有了把握，所以在做一遍会简单很多，总之，加油。

# 参考资料

- [npm是干什么的?](https://zhuanlan.zhihu.com/p/24357770)
- [前端脚手架，听起来玄乎，实际呢?](https://segmentfault.com/a/1190000016915868)
- [虚拟DOM介绍](https://juejin.im/entry/5b1496656fb9a01e28621ef0)
- [React.js 小书](http://huziketang.mangojuice.top/books/react/)
- [React中的事件处理](https://juejin.im/post/5d613dfcf265da03a53a41dc)
- [深入理解React组件状态(State)](https://www.jianshu.com/p/c6257cbef1b1)
- [重新认识生命周期函数](https://juejin.im/entry/5ba3013fe51d453eb93d53e5)
- [React中类组件和函数式组件](https://segmentfault.com/a/1190000020353236)
- [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#.kuvqndiqq)
- [使用Render Props吧](https://juejin.im/post/5a3087746fb9a0450c4963a5)
- [Hook](https://zh-hans.reactjs.org/docs/hooks-intro.html)
- [BraftEditor](https://braft.margox.cn/)