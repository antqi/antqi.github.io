## 历史由来[wiki](https://zh.wikipedia.org/wiki/CommonJS)

> CommonJS 是一个项目，创建的主要原因是当时缺乏普遍可接受形式的 JavaScript 脚本模块单元，模块在与运行 JavaScript 脚本的常规网页浏览器所提供的不同的环境下可以重复使用

2009 年 1 月由[Mozilla](https://zh.wikipedia.org/wiki/Mozilla)工程师 Kevin Dangoor 发起，起初命名“ServerJS”。在 2009 年 8 月，这个项目被改为“CommonJS”来展示其[API](https://zh.wikipedia.org/wiki/API)的广泛的应用性。

2013 年 5 月，[Node.js](https://zh.wikipedia.org/wiki/Node.js)包管理器[npm](https://zh.wikipedia.org/wiki/Npm)的作者 Isaac Z. Schlueter，宣布 Node.js 已经废弃了 CommonJS。后续，NodeJS 在计划使用 [ES6 模块系统](https://nodejs.org/api/esm.html#esm_ecmascript_modules)

JavaScript 一一直没有模块化的概念，直到 2009 年 Node.js 的出现，模块化成了必要实现。因此 Node.js 首先在服务端实现了 CommonJS 规范的模块系统。

而后[Browserify](https://github.com/browserify)实现了浏览器端的 CommonJS 规范，自此之后，使得浏览器端的代码能以以模块化方式运行，并能使用 npm 上传、维护以及使用 JavaScript 模块

## CommonJS 约定简述[Modules/1.1.1](http://wiki.commonjs.org/wiki/Modules/1.1.1)

### 首先简述 CommonJS 中的模块是什么？

每个文件都是一个模块，有自己的作用域。

每个文件里面变量、函数等等都是私有的 ，提供导出功能来选择开发哪些变量、函数等等，并可依赖其他模块。

### CommonJS 规范约定模块的实现

#### Module

每个文件可以看成一个 Module;

Module 是一个对象，

- 包含属性 id：唯一的标识符；
- 可能具备包含属性 uri，都是用来标识 module 的唯一性。

- Module 包含自由变量`require`

- Module 包含自由变量`exports`，`exports`作为唯一的导出方法，`exports`可以在执行时向其添加 API 的对象。

#### Require

用来加载需要的模块

```javascript
require(module.id / uri)
```

Require 是一个函数，接受模块标识符/路径（`module.id`/`uri`），返回外部模块导出 API。

```javascript
var path = require('path') // 标识符，一般是引用第三方模块
var myCalc = require('./tool/calc') // 路径，一般为项目自定义模块时

var result = myCalc.add(2, 3) // 使用模块开放的方法
```

#### exports

也就是向外暴露模块。

作为唯一的导出方法,可以在执行时向其添加 API 的对象。

语法上有两种方式：

###### module.exports = 任意类型

```javascript
var foo = function () {
  // 方法体
}
module.exports = foo
```

###### exports.xxx = 任意类型

```javascript
const name = 'john'

const nums = [1, 4, 3, 5]

function showName() {
  console.log(name)
}

exports.name = name
exports.nums = nums
exports.showName = showName
```

## 具体实现

有一点很重要，CommonJS 模块是同步加载的

- 服务器端，NodeJS 是 CommonJS 规范的实现 - [demo](https://github.com/antqi/test/tree/master/JavaScript模块化/5-CommonJS)

  模块加载在程序初始化的时候，会稍微慢点

- 浏览器端，Browserify 是 CommonJS 规范的实现- [demo](https://github.com/antqi/test/tree/master/JavaScript模块化/5-CommonJS)

  浏览器需要模块的时候，向服务器发送请求，服务器先合并文件中所有的模块并编译之后，再发送给浏览器。这个过程非常耗时。加上同步加载，使得模块加载的时候，无法运行后面的语句。这会导致内容从请求到展示到用户，需要的时间更长。体验非常差。这时候浏览器端的模块加载使用异步方式更优。

