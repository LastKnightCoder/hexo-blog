---
title: Webpack入门
categories: 
	- Webpack
tags:
	- Webapck
date: 2020-07-19
---

## Webpack介绍

`webpack` 是什么，以及它有什么作用，解决了什么问题，这些都是我们需要在学习 `webpack` 的用法之前需要了解的。

如果你上网去搜的话，一般的介绍是它是一个打包工具，那么什么又叫做打包工具? 由于现在的开发都是多人协作开发，所以一般会将功能分成一个个的模块由不同程序员开发，但是这些模块之间并不是毫无关联的，模块之间会有依赖关系。我们分开为了多个文件，由于依赖关系复杂，如果这些文件加载顺序不当，就有可能发生错误，另外分开成多个文件也意味着有多个网络请求，这会对服务器造成压力，所以我们就有需要将它们合并成一个文件的需要，而将多个文件合成成一个文件，我们就称之为打包。

这里贴一张官网的图片

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200715101557.png"/>

通过官网的图片可以看出，`webpack` 可以将依赖的文件打包成单个的资源文件。

但是 `webpack` 除了打包之外还有什么作用，或者说我们有什么需求需要它帮我们去完成。例如我们希望在项目中使用高级的 `JS` 语法，比如 `ES6` 或者更高级的语法，可是浏览器对于 `ES6` 的支持可能并不完全，但是我们就是想用，这个时候我们就有将 `ES6` 及更高级的语法转化为 `ES5` 或更低级语法的需要。

另外，随着项目的复杂，使用 `CSS` 编写样式不能满足日益复杂的项目的需要，或者说 `CSS` 并不是一个工程语言，这个意思指 `CSS` 难以阅读、修改、扩展。所以就出现像 `less` `scss/sass` 这样的工程化的语言，它可以很方便的写样式，并且十分方便管理，但是浏览器并不认识 `less` `scss` 等编写的样式，所以我们就有将 `less、scss/sass` 文件转化为 `CSS` 文件的需要。

再一个，我们会在项目中用到 `TypeScript`，因为 `JavaScript` 的灵活以及动态特性，导致很多的错误只有在运行时才暴露出来，所以 `JavaScript` 很容易出现 `bug`，使用 `TypeScript` 可以大大减少这种情况的发生，错误往往在编译时就能够发现，但是浏览器并不支持 `TypeScript`，所以我们有将 `TypeScript` 转化为 `JavaScript` 的需要。

其实还有很多的需求，比如压缩文件，比如为 `CSS` 样式添加浏览器前缀等等，这些等到我们后面在介绍。

## webpack安装及简易入门

新建一个项目，这里我的项目名为 `webpack-dev`

```bash
mkdir webpack-dev
cd webpack-dev
npm init -y
```

通过 `npm init` 命令我们初始化了一个 `npm` 项目，接着我们下载 `webpack` 和 `webpack-cli`

```bash
npm install webpack webpack-cli --save-dev
```

为什么是本地安装而不是全局安装，如果是全局安装的话，在不同的电脑大家的 `webpack` 的版本可能是不一样的，所以可能会因为版本的不同而带来问题，为了避免这种情况的发生，我们选择本地局部安装，版本信息会被保存在 `package.json` 中，这样大家安装时版本就会是一样的，避免了不必要的问题，这里我们 `webpack` 版本如下

```json
"webpack": "^4.43.0",
"webpack-cli": "^3.3.12"
```

下面我们新建一个 `src` 文件夹，`src` 文件夹一般是我们开发时所使用的文件夹，我们在这个文件夹下写代码，在 `src` 下新建 `index.js` 和 `util.js`，内容如下

```javascript
// index.js
import {add} from './util'

let res = add(1, 2);
console.log(res); // 3
```

```javascript
// util.js
export const add = (x, y) => {
    return x + y;
};
```

上面 `index.js` 依赖了 `util.js`，我们需要将这两个文件打包为一个文件，在命令行使用如下 `webpack` 命令进行打包

```bash
npx webpack --entry=./src/index.js --output=./dist/bundle.js --mode=development
```

在命令行有如下输出

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200717212112.png" width="70%"/>

如果出现上面的输出，说明打包成功了。接下来解释一下上面的命令：因为 `webpack` 是本地局部安装的，所以我们要使用 `npx` 来调用 `webpack` 命令，除此以外，我们还传递了三个参数

| 参数     | 功能                           |
| -------- | ------------------------------ |
| `entry`  | 指定打包的入口文件             |
| `output` | 指定打包后的文件输出在哪个目录 |
| `mode`   | 指定模式                       |

我们使用 `entry` 来指定打包的入口文件，即 `webpack` 会根据该入口文件，解析它所依赖的文件，将这些文件打包成一个文件；我们使用 `output` 来表明所打包后的那个文件放置在哪个目录，以及打包后的文件的文件名是什么，在上面我们设置在打包后的文件放在 `dist` 文件夹，文件名为 `bundle.js`，使用上面的命令之后，可以观察到自动生成了一个 `dist` 文件夹，以及在该文件夹下有一个 `bundle.js` 的文件

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200717210455.png" width="40%"/>

`mode` 是用来指定模式的，可选的选项有 `development` 和 `production`，`development` 指的是开发环境，这个时候我们打包出来的代码不会被压缩，当指定 `mode` 为 `production` 时，即生产环境下，那么打包后的代码会被压缩。

在上面我们使用的是 `ES6` 的模块语法，即 `import` 和 `export`，但是很多的 `npm` 项目使用的是 `CommonJS` 模块规则，即 `require` 和 `module.exports`，所以在项目中很可能我们会同时使用上面两种模块进行导出和导入，这样的代码是不可能在浏览器中执行的，使用 `webpack` 它可以处理这种情况，我们可以在项目中自由的使用这两种导入导出的规则，`webpack` 会自动帮我们处理为可以在浏览器中执行的代码，在 `src` 下新建 `foo.js` 如下

```javascript
// foo.js
module.exports = {
    sub: (x, y) => {
        return x - y;
    }
};
```

并且修改 `index.js` 如下

```javascript
// index.js
import {add} from './util'
const sub = require('./foo').sub;

let res1 = add(1, 2);
let res2 = sub(1, 2);

console.log(res1); // 3
console.log(res2); // -1
```

在 `index.js` 中，我们同时应用了两种模块语法，我们在命令行中再次执行

```bash
npx webpack --entry=./src/index.js --output=./dist/bundle.js --mode=development
```

为了观察打包后的文件是否能够在浏览器执行，我们在 `dist` 下新建 `index.html` 并引用 `bundle.js`，如下

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script src="bundle.js"></script>
</body>
</html>
```

在浏览器打开 `index.html`，在控制台可以观察到如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200717212542.png" width="50%"/>

可见 `webpack` 为我们自动处理了这种语法。

> 为了不每次在命令行输入上面一行特长的命令，我们可以配置 `package.json` 中的 `script` 属性，如下
>
> ```json
> "scripts": {
>   "build": "webpack --entry=./src/index.js --output=./dist/bundle.js --mode=development",
> }
> ```
>
> 下面我们只要在命令行输入
>
> ```bash
> npm run build
> ```
>
> 它就会执行那一串命令，即可进行打包。

## 配置文件

在上面的简易入门中，我们没有为 `webpack` 写配置文件，只是在命令行中设置了 `entry` `output` `mode` 这些参数，但是其实这些参数都有默认值

| 参数     | 默认值         |
| -------- | -------------- |
| `entry`  | `src/index.js` |
| `output` | `dist/main.js` |
| `mode`   | `production`   |

这意味着即使我们在命令行不配置这些参数，也能够进行正常的打包，会在 `dist` 目录下生成一个 `main.js` 文件而不是 `bundle.js` 文件。所以可以说 `webpack` 是零配置，不需要配置对用户来说是一件好事，可以减少很多的心智负担，但是零配置也意味着功能较弱，为了使用 `webpack` 强大的功能，我们必须要学习 `webpack` 许许多多的配置，但是这些配置都写在命令行中，那是十分的不方便，这些东西应该放在配置文件中。

`webpack` 的默认配置文件名为 `webpack.config.js` 或者 `webpackfile.js`，由于业界大多数使用的都是 `webpack.config.js`，所以我们在这里也使用 `webpack.config.js`。在项目的根目录(而不是 `src` 目录)下新建 `webpack.config.js`，内容如下

```javascript
const path = require('path');
module.exports = {
    entry: "./src/index.js",
    output: {
        filename: "bundle.js",
        path: path.resolve(__dirname, "dist")
    },
    mode: "development"
};
```

这时我们就不必在命令行中指定参数了，我们修改 `package.json` 的 `script` 字段如下

```json
"scripts": {
  "build": "webpack"
}
```

接着在命令行运行 `npm run build` 即可看到打包成功的消息。下面就更加详细的讲解一下 `entry` 和 `output` 的配置。

### entry

`entry` 指代的就是入口文件，这是我们在上面就已经介绍过的。其实 `entry` 可以有多个入口，`webpack` `webpack` 会根据每一个入口文件打包出一个 `chunk`

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200718152109.png"/>

这个 `chunk` 可以有名字，当如下配置时

```javascript
entry: "./src/index.js"
```

打包出的 `chunk` 的名字默认为 `main`。我们也可以使用对象的方式配置 `chunk` 的名字

```javascript
entry: {
    index: "./src/index.js" // index 表示 chunk 名字，对应的值表示对应的入口文件
},
output: {
    filename: "[name].js",
    path: path.resolve(__dirname, "dist")
},
```

在上面有两处改动，第一个就是将 `entry` 的值设置为了一个对象，该对象的值表示入口文件，而键表示入口文件打包后的 `chunk` 的名字；第二个改动就是在 `output` 中我们设置打包后的文件名称为 `[name].js`，这个 `[name]` 是一个变量，指的就是 `chunk` 的名字，我们没有手动的写死一个值，而是使打包后的文件名与 `chunk` 名字相同，所以此时运行 `npm run build` 可以在 `dist` 目录下打包出 `index.js` 文件。下面是运行打包命令在命令行的输出

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200718150150.png" width="80%"/>

### output

`output` 的常见配置选项如下

| 选项         | 功能                                                       |
| ------------ | ---------------------------------------------------------- |
| `filename`   | 指定文件名称，可以为 `[name]` `[chunkHash]` 以及它们的组合 |
| `path`       | 打包后的文件放置的路径                                     |
| `publicPath` |                                                            |

`filename` 是用来指定打包后的文件名的，我们除了可以指定特定的名称如 `bundle.js` 外，还可以使用 `[name]` `[chunkHash]` 等变量，分别指打包后的 `chunk` 的名称和打包后文件内容计算出的 `hash` 值。二者可以进行组合使用，如

```javascript
output: {
    filename: "[name][chunkHash].js",
    path: path.resolve(__dirname, "dist")
}
```

## 本地服务器

上面我们的开发过程为

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200718162136.png" width="80%"/>

每次我们更改程序后，都要重新刷新网页以观察最新的结果，而我们希望在我们更改程序后，页面能够自动的刷新，不需要再次使用 `webpack` 进行打包，然后手动刷新页面，这个时候我们就需要使用 `webpack-dev-server` 了，它可以在本地开启一个服务器，并且当我们修改程序时，自动的刷新页面。

首先下载 `webpack-dev-server`

```bash
npm install webpack-dev-server --save-dev
```

接着我们在 `webpack.config.js` 中加入 `devServer` 配置项

```javascript
const path = require('path');
module.exports = {
    // webpack-dev-server 的配置
    devServer: {
        port: 3000,
        contentBase: path.resolve(__dirname, 'dist')
    },
    entry: {
        "bundle": "./src/index.js"
    },
    output: {
        filename: "[name].js",
        path: path.resolve(__dirname, "dist")
    },
    mode: "development"
};
```

在上面我们配置了两个选项

| 选项          | 功能               |
| ------------- | ------------------ |
| `port`        | 配置端口号         |
| `contentBase` | 配置服务器的根目录 |

接着我们为 `package.json` 中的 `script` 字段添加 `dev` 选项

```json
"scripts": {
  "build": "webpack",
  "dev": "webpack-dev-server"
}
```

接着我们运行 `npm run dev` 即可在本地启动一个服务器了

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200718192451.png"/>

接着我们在浏览器输入 `http://localhost:3000` 即可访问页面了。

> 注意：
>
> 这里有一点需要注意的是，使用 `webpack-dev-server` 并不会输出打包后的文件，它会根据入口文件的依赖关系进行打包，但是打包后的内容放在内存中，而不是输出一个文件到硬盘上，所以不会生成 `bundle.js` 文件，因为它只存在于内存中。

## loader

`loader` 又是一个新的概念，我们可以简单的理解为处理器，处理什么呢? 比如我们需要将 `ES6` 转换为 `ES5`，这就需要一个 `loader` 进行转换；比如我们需要将 `less` 转换为 `CSS`，这需要一个 `loader` 进行转换，比如需要将 `TypeScript` 转换为 `JavaScript`，这也需要一个 `loader` 进行转换；等等。

下面我们就介绍一些常用的 `loader`。

### babel-loader

我们常常有需要将 `ES6` 或更高级的语法转化为 `ES5`的需要，这个时候我们就要用到 `babel-loader`，我们需要下载这些安装包

```bash
npm install babel-loader @babel/core --save-dev
```

`@babel/core` 是 `babel` 的核心包，具体的转换要依赖该包，接着我们需要在 `webpack.config.js` 中进行配置

```javascript
const path = require('path');
module.exports = {
    devServer: {
        port: 3000,
        contentBase: path.resolve(__dirname, 'dist')
    },
    entry: {
        "bundle": "./src/index.js"
    },
    output: {
        filename: "[name].js",
        path: path.resolve(__dirname, "dist")
    },
    mode: "development",
    module: {
        rules: [
            // 配置 loader
            {
                test: /\.js$/,
                use: "babel-loader"
            }
        ]
    }
};
```

我们在 `module.rules` 中配置 `loader`，其中最少需要配置两项 `test` 和 `use`，`test` 用来表示对什么文件进行转换，比如上面对所有以 `.js` 结尾的文件进行转换；`use` 用来指定使用什么 `loader` 进行转换，这里指定为 `babel-loader`。综合上面两个配置，表示的意思就是对所有以 `.js` 结尾的文件使用 `babel-loader` 进行处理。

其中 `use` 也可以是一个对象，如下

```javascript
use: {
    loader: "babel-loader",
    options: {
        
    }
}
```

当 `use` 是一个对象时，我们可以通过 `options` 为 `loader` 配置一些参数。

除了配置 `test` 和 `use`，还可以配置两个选项

| 选项      | 功能           |
| --------- | -------------- |
| `include` | 指定检查的路径 |
| `exclude` | 指定排除的路径 |

上面的两个选项指定只对哪些路径下的文件进行检查，或者不检查哪些路径下的文件，一般来说二者只需要配置一个即可，但是如果两个都配置了，并且有冲突，那么 `exclude` 的优先级高。

我们接着修改 `index.js` 的内容，为其中添加 `ES6` 的语法

```javascript
let x = 1;
let y = 2;
const add = (x, y) => {
    return x + y;
};

let result = add(1, 2);

let p = new Promise((resolve, reject) => {
    resolve(1);
}).then(value => {
    console.log(value);
});
```

在 `index.js` 中，我们使用了 `ES6` 才出现的语法，如 `let` `const` `Promise` 箭头函数，现在我们运行 `npm run build` 进行打包，部分内容如下

```javascript
eval("let x = 1;\nlet y = 2;\n\nconst add = (x, y) => {\n  return x + y;\n};\n\nlet result = add(1, 2);\nlet p = new Promise((resolve, reject) => {\n  resolve(1);\n}).then(value => {\n  console.log(value);\n});\n\n//# sourceURL=webpack:///./src/index.js?");
```

发现并没有将 `ES6` 的语法转化为 `ES5` 的语法，因为 `babel` 需要特定的插件支持，`babel` 为了实现按需加载的功能，它将 `ES6` 转 `ES5` 的功能分为了 `20+` 个插件，你需要哪个就下哪个，但是这样却是有点麻烦，好在 `babel` 推出了一个插件集合 `preset`，我们直接下载 `@babel/preset-env` 即可，里面包含了所有的 `ES6` 语法转 `ES5` 语法的插件合集

```bash
npm install @babel/preset-env --save-dev
```

接着我们在 `webpack.config.js` 中进行配置

```javascript
test: /\.js$/,
use: {
    loader: "babel-loader",
    options: {
        "presets": [
            "@babel/preset-env"
        ]
    },
    exclude: path.resolve(__dirname, "node_modules/") // 对 node_modules 中的文件不进行转换
}
```

但是观察打包后的文件，`let` `const` 和箭头函数都被转为了 `ES5`，但是 `Promise` 并没有被转换，因为这是新增的 `API` 而不是语法，这种新增的 `API` 靠 `preset-env` 是转化不了的，它只能转化语法，所以 `class` 以及 `Object.assign` 等这些新增的 `API` 它是不能转换的，如果在版本比较低的浏览器中，由于不支持这些 `API`，那么就会报错。

这个时候需要 `polyfill` 为浏览器提供 `ES6` 的环境，即使用 `ES5` 或更低的语法去实现上面的 `API`，有两种解决办法。

1. 下载 `@babel/polyfill`，并且在入口文件即 `index.js` 中引入

   ```bash
   npm install @babel/polyfill --save
   ```

   接着在 `index.js` 中引入

   ```javascript
   // 引入 @babel/polyfill
   import '@babel/polyfill'
   
   let x = 1;
   let y = 2;
   const add = (x, y) => {
       return x + y;
   };
   
   let result = add(1, 2);
   
   let p = new Promise((resolve, reject) => {
       resolve(1);
   }).then(value => {
       console.log(value);
   });
   ```

   `@babel/polyfill` 使用 `ES5` 的语法实现低版本浏览器中没有的 `API`，所以即使在低版本的浏览器中也能够运行上面的程序，但是 `@babel/polyfill` 同时也覆盖了原有的 `API`，另外我们直接导入了 `@babel/polyfill` 中的所有内容，其中有一些 `API` 我们根本没有用到，比如 `class`，这就会导致打包出的文件体积较大。

2. 为了实现按需加载，我们使用另一种解决办法，下载一个插件 `@babel/plugin-transform-runtime`，因为 `@babel/plugin-transform-runtime` 依赖于 `@babel/runtime`，所以我们也要下载 `@babel/runtime`，并且 `@babel/runtime` 生产环境下也要用到，所以不能使用 `--save-dev` 安装

   ```bash
   npm install @babel/plugin-transform-runtime --save-dev
   npm install @babel/runtime
   ```

   接着我们在 `webpack.config.js` 中加入插件的配置

   ```javascript
   test: /\.js$/,
   use: {
       loader: "babel-loader",
       options: {
           "presets": [
               "@babel/preset-env"
           ],
           "plugins": [
               "@babel/plugin-transform-runtime"
           ]
       }
   }
   ```

   接着我们去掉 `index.js` 头部添加的 `import '@babel/polyfill'`，再次使用 `webpack` 进行打包即可。

### css-loader、style-loader

除了可以对 `JavaScript` 进行打包以外，还可以对 `CSS` 进行打包，不过 `webpack` 只能处理 `JavaScript`，即不能把 `CSS` 文件当做入口文件进行打包，不过我们可以在 `JavaScript` 中导入 `CSS`，当对 `JavaScript` 进行打包，也会同时对导入的 `CSS` 进行打包，在 `src` 中新建 `index.css` 和 `foo.css`

```css
/* index.css */
@import "foo.css";
body {
    background-color: #5d4141;
}
```

```css
/* foo.css */
body {
    color: white;
    font-size: 4em;
}
```

我们在 `index.css` 中通过 `@import` 语法引入了 `foo.css`，接着我们在 `index.js` 中引入 `index.css`

```javascript
import './index.css'

document.write("Hello World");
```

我们通过 `import './index.css'` 引入了 `CSS`，`webpack` 将一切都看作是模块，`CSS` 文件也可以看做是模块，我们就可以通过 `import` 将该模块导入。

我们希望有一个 `loader` 来处理 `import './index.css'` 的语法，使样式生效，这里我们需要两个 `loader`，分别为 `css-loader` 和 `style-loader`，首先进行安装

```bash
npm install css-loader style-loader --save-dev
```

`css-loader` 的作用是处理 `CSS` 文件中 `@import` 和 `url()` 这样的语法的，而 `style-loader` 是将解析完的 `CSS` 样式作为 `style` 标签插入到页面的 `head` 元素中。我们在 `webpack.config.js` 对 `loader` 进行配置

```javascript
test: /\.css$/,
use: ["style-loader", "css-loader"]
```

`loader` 的执行是有先后顺序，`webpack` 会根据 `use` 中定义的 `loader` 从后往前执行，所以我们把 `css-loader` 写在 `style-loader` 后面，这样就会先执行 `css-loader`，然后执行 `style-loader`。

我们运行 `npm run dev`，在 `http://localhost:3000` 可以观察到下面的页面

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200719104455.png" width="80%"/>

说明我们的配置成功了。

除了处理 `CSS` 文件，有时候我们会使用 `less` `sass/scss` 编写样式，但是这些文件浏览器不能解析，所以我们需要使用相应的 `loader` 将 `less` `sass/scss` 转为 `CSS` 文件，例如 `less-loader` `sass-loader`

```bash
npm install less less-loader node-sass sass-loader --save-dev
```

接着我们在 `webpack.config.js` 中添加相应的配置

```javascript
{
    test: /\.less$/,
    use: ["style-loader", "css-loader", "less-loader"]
},
{
    test: /\.(sass|scss)$/,
    use: ["style-loader", "css-loader", "sass-loader"]
}
```

`less-loader` 和 `sass-loader` 只是把要处理的文件转化为 `CSS` 文件，最后我们还是需要通过 `css-loader` 和 `style-loader` 来处理路径以及将 `CSS` 插入到页面中。

### file-loader、url-loader

下面介绍有关处理图片相关的 `loader`，假设在 `index.js` 的中引用了图片

```javascript
let img = new Image();
img.src = "./beauty.jpg";
document.body.appendChild(img);
```

我们进行打包，发现在浏览器中并没有出现我们想要的图片，因为我们在文件中引用的图片是在 `src` 目录下的，在 `dist` 目录下并没有这张图片，所以我们也需要将图片打包到 `dist` 目录下，如下

```javascript
import beauty from './beauty.jpg'
let img = new Image();
img.src = beauty;
document.body.appendChild(img);
```

我们将图片作为模块导入，希望得到该图片的打包后的地址，然后将 `img.src` 设置为这个地址。这件事情需要一个 `loader` 去做，这里我们使用 `file-loader`，它的作用就是将引入的文件(一般是图片)打包到(或者说移动到) 指定目录下，并返回打包后的文件所在的路径，我们需要安装 `file-loader`

```bash
npm install file-loader --save-dev
```

并在 `webpack.config.js` 中进行配置

```javascript
{
    test: /\.(png|jpe?g|gif|svg)$/,
    use: {
        loader: "file-loader",
        options: {
            name: "[name].[ext]",
            outputPath: "img/"
        }
    }
}
```

其中 `name` 是用来配置输出的文件名的，这里我们使用了两个变量 `[name]` 和 `[ext]`，`[name]` 代表打包前文件的名称，`[ext]` 表示扩展名；`outputPath` 用来指定打包后文件输出的位置，这个位置是相对于 `output.path` 的路径。所以会在 `dist/img` 下打包出一张与原文件名相同的一张图片，并且将该打包后的图片的地址返回，这样在页面中就会出现图片。

有时候对于较小的图片，我们希望将它转为 `base64`，这样可以减少 `http` 请求次数。我们使用 `url-loader` 来做这个事情，它提供了一个 `limit` 选项，该选项指定一个值，当图片的大小小于该值时就转化为 `base64`，当大于该值时，就使用 `file-loader` 进行转换，我们修改 `webpack.config.js` 的配置

```javascript
test: /\.(png|jpe?g|gif|svg)$/,
use: {
    loader: "url-loader",
    options: {
        name: "[name].[ext]",
        limit: 200*1024 // 小于 200KB 就转化为 base64 编码
    }
}
```

### ts-loader

我们在项目中也经常使用 `TypeScript` 来写程序，因为 `TypeScript` 的种种好处，可以帮我们减少 `bug` 产生的几率，但是浏览器也是不认识 `TypeScript`，我们需要将 `TypeScript` 转化为 `JavaScript`，这需要 `ts-loader` 来帮我们做这件事情，首先我们需要下载相关的包

```bash
npm install typescript ts-loader --save-dev
```

接着我们需要配置 `webpack.config.js` 以及 `tsconfig.json`(在项目根目录下新建该文件)

```javascript
test: /\.ts$/,
use: "ts-loader"
```

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es5",
    "allowJs": true
  },
  "include": [
    "./src/"
  ],
  "exclude": [
    "./node_modules/"
  ]
}
```

关于 `tsconfig.json` 的配置不是重点，可以去网上了解相关内容。现在我们就可以自由的写 `TypeScript` 了。

## plugin

`plugin` 是 `webpack` 的另一大特色，它为我们提供了相当多的功能，在 `webpack` 打包的整个生命周期中会广播出很多事件，而 `plugin` 可以监听这些事件，在合适的时机通过 `webapck` 提供的 `API` 改变输出的结果。下面就介绍一些常用到的 `plugin`。

### mini-css-extract-plugin

在处理 `CSS` 的 `loader` 中，我们最后使用 `style-loader` 将最后的样式以 `style` 标签的形式插入到页面中，但是如果我们希望将样式抽离出一个 `CSS` 文件呢? 这个时候我们就需要用到 `mini-css-extract-plugin` 插件了。首先进行下载

```bash
npm install mini-css-extract-plugin --save-dev
```

接着在 `webpack.config.js` 中进行配置

```javascript
const path = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
    // 省略其他配置
    module: {
        rules: [
            // 省略其他 loader 配置
            {
                test: /\.css$/,
                use: [MiniCssExtractPlugin.loader, "css-loader"]
            }
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            filename: "main.css" // 抽离出的 css 文件名称
        })
    ]
};
```

首先我们通过 `require('mini-css-extract-plugin')` 引入该插件，接着在 `plugins` 选项中配置了该插件，并设置了抽离后的 `CSS` 文件的名称，接着因为我们希望最后抽离出一个 `CSS` 文件而不是将文件以 `style` 标签的形式插入到页面中，所以我们使用了 `MiniCssExtractPlugin.loader` 来替换了 `style-loader`。

我们运行 `npm run build` 就可以观察到在 `dist` 目录下生成了一个 `main.css` 文件。为了观察到效果，我们在 `dist/index.html` 中将该 `main.css` 引入

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<link rel="stylesheet" href="./main.css">
<script src="bundle.js"></script>
</body>
</html>
```

打开该 `html` 就可以看到效果了。

### clean-webpack-plugin

该插件的作用是在每次打包前删除 `dist` 文件夹，这样我们每次都可以看到最新的打包文件。可能你还不能理解这个应用场景，现在我们修改 `output.filename` 如下

```javascript
output: {
    filename: "[name]@[chunkHash].js",
    path: path.resolve(__dirname, "dist")
}
```

每次生成的文件由名字和文件内容生成的 `hash` 值组成，每次我们修改程序，然后打包，就会产生不同的哈希值，所以 `dist` 下的文件会不断的增多，我们根本不知道哪个文件是我们最新打包出来的，如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200719153829.png" width="40%"/>

所以我们就有这个需要，在生成新的打包文件之前，将之前的打包文件进行删除。首先下载 `clean-webpack-plugin`

```bash
npm install clean-webpack-plugin --save-dev
```

接着配置 `webpack.config.js`

```javascript
// 省略其他
const {CleanWebpackPlugin} = require('clean-webpack-plugin');
module.exports = {
    // 省略其他配置
    plugins: [
        new MiniCssExtractPlugin({
            filename: "main.css"
        }),
        new CleanWebpackPlugin()
    ]

};
```

再次进行打包，发现之前的 `dist` 文件夹下之前的打包文件都被删掉了。

### html-webpack-plugin

使用 `clean-webpack-plugin` 会将我们之前在 `dist` 下新建的 `index.html` 文件也会删除掉，为了观察打包效果，我们不得不在 `dist` 目录下再次新建 `index.html` 并且将打包后的 `JavaScript` 文件和 `CSS` 文件引入，每次打包一次都要这么做实在令人恼火，我们使用 `html-webpack-plugin` 来解决这个问题。

`html-webpack-plugin` 会根据一个 `html` 文件模板，在 `dist` 文件夹下生成一个 `html` 文件，并且会将打包后的 `JavaScript` 和 `CSS` 自动引入，大大的解放了我们的双手。首先我们下载 `html-webpack-plugin`

```bash
npm install html-webpack-plugin --save-dev
```

接着在 `webpack.config.js` 中进行配置

```javascript
// 省略其他
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
    // 省略其他配置
    plugins: [
        new MiniCssExtractPlugin({
            filename: "main.css"
        }),
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            template: "./src/index.html",
            filename: "index.html"
        })
    ]
};
```

其中我们在 `HtmlWebpackPlugin` 中传入了两个配置项

| 配置项     | 作用                           |
| ---------- | ------------------------------ |
| `template` | 指定模板的路径                 |
| `filename` | 指定生成的 `html` 文件的文件名 |

在上面，我们将模板指定为 `src/index.html`，所以我们在 `src` 新建 `index.html` 如下

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
</html>
```

只是一个标准的 `html` 文件，里面什么都没有。运行 `npm run build` 即可观察到在 `dist` 文件夹下生成了一个 `index.html` 文件

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200719160224.png" width="40%"/>

生成的 `index.html` 的内容如下

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
<link href="main.css" rel="stylesheet"></head>
<body>

<script src="bundle%4063ea546939ae13c70c31.js"></script></body>
</html>
```

可见它已经自动为我们引入了打包后的 `JavaScript` 文件和 `CSS` 文件。

## 参考文章

- [webpack loader和plugin编写](https://juejin.im/post/5bbf190de51d450ea52fffd3)
- [Webpack配置Typescript](https://juejin.im/post/5d6f685df265da03bf0f616b)
- [你真的会用 Babel 吗?](https://segmentfault.com/a/1190000011155061)
- [@babel/preset-env, @babel/polyfill和@babel/plugin-transform-runtime](https://www.tangshuang.net/7427.html)
- [webpack 4.0](https://www.bilibili.com/video/BV1QE411M7sj)
- [Babel：plugin、preset的区别与使用](https://juejin.im/entry/5b15d3acf265da6e38191f80)
- [webpack4.0 clean-webpack-plugin 插件跳坑指南](https://juejin.im/post/5d107a56e51d45773d46863c)