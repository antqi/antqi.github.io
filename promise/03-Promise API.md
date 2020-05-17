### Promise构造函数

``` javascript
Promise (excutor){}
```

> excutor会在Promise内部立即同步回调，异步操作在执行器中执行

- excutor函数：同步执行 ( resolve , reject ) => { }

- resolved函数：内部定义成功时，调用函数 value => { }

- rejected函数：内部定义失败时，调用函数 reason => { }

  

### Promise.prototype.then

``` javascript
(onResolved,onRejected) => { }
```

> 指定用于得到成功value的成功回调和用户得到失败的reason的失败回调返回一个新的promise对象

- onResovled函数：成功的回调函数 (value) => { }

- onRejected函数：失败的回调函数 (reason) => { }

  

### Promise.prototype.catch

``` javascript
(onRejected) => { }
```

> then() 的语法糖，相当于：then ( undefined , onRejected ) => { }

- onRejected函数：失败的回调函数 ( reason ) => { }

### Promise.resolve

``` javascript
(value) => { }
```

> 返回一个成功/失败的Promise对象

- value : 成功的数据或promise对象

   

### Promise.reject 

``` javascript
(reason) => { }
```

> 返回一个失败的Promise对象

- reason ：失败的原因



### Promise.all 

``` javascript
( promises ) => { }
```

> 返回一个新的Promise，只有所有的Promise都成功才成功，只要有一个失败了就直接失败

- promises ：包含 N 个promise的数组



### Promise.race 

``` javascript
( promises ) => { }
```

> 返回一个新的promise ，第一个完成的promise的结果状态就是最终的结果状态

- promises ：包含 N 个promise的数组

