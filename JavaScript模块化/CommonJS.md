## 历史由来[wiki](https://zh.wikipedia.org/wiki/CommonJS)

> CommonJS是一个项目，创建的主要原因是当时缺乏普遍可接受形式的JavaScript脚本模块单元，模块在与运行JavaScript脚本的常规网页浏览器所提供的不同的环境下可以重复使用

2009年1月由[Mozilla](https://zh.wikipedia.org/wiki/Mozilla)工程师Kevin Dangoor发起，起初命名“ServerJS”。在2009年8月，这个项目被改为“CommonJS”来展示其[API](https://zh.wikipedia.org/wiki/API)的广泛的应用性。

2013年5月，[Node.js](https://zh.wikipedia.org/wiki/Node.js)包管理器[npm](https://zh.wikipedia.org/wiki/Npm)的作者Isaac Z. Schlueter，宣布Node.js已经废弃了CommonJS。后续，NodeJS在计划使用 [ES6模块系统](https://nodejs.org/api/esm.html#esm_ecmascript_modules)

JavaScript一一直没有模块化的概念，直到2009年Node.js的出现，模块化成了必要实现。因此Node.js首先在服务端实现了CommonJS规范的模块系统。

而后[Browserify](https://github.com/browserify)实现了浏览器端的CommonJS规范，自此之后，使得浏览器端的代码能以以模块化方式运行，并能使用npm上传、维护以及使用JavaScript模块。



## CommonJS约定简述[Modules/1.1.1](http://wiki.commonjs.org/wiki/Modules/1.1.1)

### 首先简述CommonJS中的模块是什么？

每个文件都是一个模块，有自己的作用域。

每个文件里面变量、函数等等都是私有的 ，提供导出功能来选择开发哪些变量、函数等等，并可依赖其他模块。



### CommonJS规范约定模块的实现

#### Module

每个文件可以看成一个Module;

Module是一个对象，

- 包含属性id：唯一的标识符；
- 可能具备包含属性uri，都是用来标识module的唯一性。

- Module包含自由变量`require`

- Module包含自由变量`exports`，`exports`作为唯一的导出方法，`exports`可以在执行时向其添加API的对象。

#### Require

用来加载需要的模块

``` javascript
require(module.id/uri)
```

Require是一个函数，接受模块标识符/路径（`module.id`/`uri`），返回外部模块导出API。

``` javascript
var path = require('path') // 标识符，一般是引用第三方模块
var myCalc = require('./tool/calc') // 路径，一般为项目自定义模块时

var result = myCalc.add(2,3) // 使用模块开放的方法
```

#### exports

也就是向外暴露模块。

作为唯一的导出方法,可以在执行时向其添加API的对象。

语法上有两种方式：

###### module.exports = 任意类型

``` javascript
var foo = function(){
  // 方法体
}
module.exports = foo 
```

###### exports.xxx = 任意类型

``` javascript
const name="john"

const nums=[1,4,3,5]

function showName(){
  console.log(name)
}

exports.name=name
exports.nums=nums
exports.showName=showName
```



## 具体实现

有一点很重要，CommonJS模块是同步加载的

- 服务器端，NodeJS是CommonJS规范的实现 - [demo](https://github.com/antqi/test/tree/master/JavaScript模块化/5-CommonJS)
- 浏览器端，Browserify是CommonJS规范的实现- [demo](https://github.com/antqi/test/tree/master/JavaScript模块化/5-CommonJS)

