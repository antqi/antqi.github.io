

## AMD 是什么

[AMD](https://github.com/amdjs/amdjs-api/wiki)是Asynchronous Module Definition的缩写，异步模块定义。制定了模块异步加载的规则，这样模块和模块的依赖可以异步加载。

除了有模块化本身解偶等优点之外，它的模块异步加载定义，解决了在浏览器端实现的CommonJS规范的模块同步加载导致的加载阻塞、性能差、体验差等等问题。 非常适用于浏览器环境的JavaScript模块化方案。

#### 支持AMD规范的脚本或者框架

##### 浏览器

- [RequireJS](http://requirejs.org/) 
- [curl.js](https://github.com/cujojs/curl)
- [pinf](https://github.com/pinf/loader-js)
- ...

##### 服务器

- [RequireJS](http://requirejs.org/) 
- [pinf](https://github.com/pinf/loader-js)

## AMD 规范说了什么

### define()函数

规范只定义了一个全局的函数`define`

``` javascript
define(
    module_id /*可选的*/,
    [dependencies] /*可选的*/,
    factory function /*用来实例化模块或者对象的方法*/
)
```

### module_id

module_id指定义模块中的名字，是模块的唯一标识，该参数可选。默认为脚本的文件名称。如果定义了，module_id必须是唯一的。id的命名格式参考[CommonJS-Module_Identifiers](http://wiki.commonjs.org/wiki/Modules/1.1.1#Module_Identifiers)，因为AMD模块命名规范为CommonJS的超集。

通常的理解，module_id分为两种情况：

- 普通字符串

- URI

  包含模块所在路径的字符串。模块名的相对路径解析参考[UNIX路径]([https://zh.wikipedia.org/wiki/%E8%B7%AF%E5%BE%84_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)](https://zh.wikipedia.org/wiki/路径_(计算机科学)))

因为是可选参数，当module_id不存在的时候，此模块称为**匿名模块**

### [dependencies]

dependencies表示依赖，是模块所依赖的所有模块的集合，以数组形式展现。

### definition function



如何实现的加载



- AMD 原理
- AMD 的特点
- AMD 相对于 CommonJS 的优缺点

## AMD模块加载器

## AMD 的实现（RequireJS）

- NoAMD
- RequireJS

