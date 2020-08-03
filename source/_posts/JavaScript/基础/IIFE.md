### IIFE概念

*Immediately-Invoked Function Expression*

匿名函数自调用执行

``` javascript
(function(){
  console.log('IIFE')
})()
```

### 作用

- 不会污染全局命名空间

- 隐藏实现

- 实现模块化

  现代模块化的开端

  ``` javascript
  (function(env){
    // ...
    window.$=env
  })(env)
  ```

  

