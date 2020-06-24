### 1. 如何改变promise的状态？

##### resolve(value) 

如果当前时pending就会变为resolved

##### reject(reason) 

如果当前时pending就会变为rejected

##### 抛出异常

如果当前时pending就会变为rejected

``` javascript
const p = new Promise((resolve,reject)=>{
  // throw new Error('错了') // rejected 状态，reason为error
  throw 3  // rejected 状态，reason为3
})
```



### 2. 一个promise指定多个成功/失败回调函数，会调用吗？

当promise改变为对应状态时都会调用

``` javascript
const p = new Promise((resolve,reject)=>{
  // throw new Error('错了') // rejected 状态，reason为error
  throw 3  // rejected 状态，reason为3
})

p.then(
  value => {},
  reason => { console.log('a',reason) }  
)

p.then(
  value => {},
  reason => { console.log('b',reason) }
)

// 结果 a 3 ,b 3  定义的回调函数都会执行
```



### 3. 改变promise状态和指定回调函数谁先谁后？

> 都有可能

- ##### 都有可能，正常情况下时先指定回调再改变状态，但也可以先改状态再指定回调

  ``` javascript
  new Promise((resolve,reject)=>{
    setTimeout(()=>{
      resolve(1) // 后改变的状态（同时指定数据），异步执行回调函数
    },1000)
  }).then( // 先指定回调函数，保存当前指定的回调函数
     value => { console.log(value)  },
     reason => { console.log(reason) }
  )
  ```

- ##### 如何先改变状态再指定回调

  1. 在执行器中直接调用resolve() / reject()

     ``` javascript
     new  Promise((resolve,reject) => {
       resolve(1) // 先改变状态（同时指定数据）
     }).then( // 后指定回调函数，异步执行回调函数
     	value => { console.log(value) }
     )
     ```

  2. 延迟更长时间才调用then()

     ``` javascript
     const p = new  Promise((resolve,reject) =>{
       resolve(1) // 先改变状态（同时指定数据）
     })
     
     setTimeout(()=>{ // 后指定回调函数，异步执行回调函数
       p.then( 
           value => { console.log(value)  },
         	reason => { console.log(reason) }
        )
     },1000)
     ```

- ##### 什么时候才能得到函数？

  1. 如果先指定的回调，那当状态发生改变时，回调函数就会调用，得到数据
  2. 如果先改变的状态，那当指定回调时，回调函数就会调用，得到函数

  

### 4. promise.then( ) 返回的新promise的结果状态由什么决定？

- ##### 简单表达

  <u>*由then( )指定的回调函数执行结果决定*</u>

- ##### 详细表达

  1. 如果抛出异常，新的promise变为rejected，reason为抛出异常
  
     ``` javascript
     new Promise((resolve, reject) => {
         // resolve(0)
         // reject(1)
         throw 0
       })
         .then(
           (value) => {
             console.log('onResolved 结果状态1:', value)
             throw 'onResolved1 抛出了一个异常'
           },
           (reason) => {
             console.log('onRejected 结果状态1:', reason)
             throw 'onRejected1 抛出了一个异常'
           }
         )
         .then(
           (value) => {
             console.log('onResolved 结果状态2:', value)
           },
           (reason) => {
             console.log('onRejected 结果状态2:', reason)
           }
         )
     ```
  
  2. 如果返回的是非promise的任意值，新的promise变为resolved，value为返回的值
  
     ``` javascript
     new Promise((resolve, reject) => {
         // resolve(0)
         reject(0)
         // throw 0
       })
         .then(
           (value) => {
             console.log('onResolved 结果状态1:', value)
             return 'onResolved1 1'
           },
           (reason) => {
             console.log('onRejected 结果状态1:', reason)
             return 'onRejected1 1'
           }
         )
         .then(
           (value) => {
             console.log('onResolved 结果状态2:', value)
           },
           (reason) => {
             console.log('onRejected 结果状态2:', reason)
           }
         )
     ```
  
  3. 如果返回的是另一个新的promise，此promise的结果就会成为新promise的结果
  
     ``` javascript
       new Promise((resolve, reject) => {
         const title = '如果返回的是另一个新的promise'
         // resolve(title)
         reject(title)
         // throw title
       })
         .then(
           (value) => {
             console.log('onResolved 结果状态1:', value)
             return new Promise((resolve, reject) => {
               setTimeout(() => {
                 resolve('onResolved1的Promise')
               }, 100)
             })
           },
           (reason) => {
             console.log('onRejected 结果状态1:', reason)
             return new Promise((resolve, reject) => {
               setTimeout(() => {
                 resolve('onRejected1的Promise')
               }, 100)
             })
           }
         )
         .then(
           (value) => {
             console.log('onResolved 结果状态2:', value)
           },
           (reason) => {
             console.log('onRejected 结果状态2:', reason)
           }
         )
     ```
  
     

### 5. promise如何串连多个操作任务

1. promise的then( )返回一个新的promise，可以开成then( )的链式调用

2. 通过then的链式调用<u>串连</u>多个同步/异步任务

   - 同步: return  任意数据
- 异步: return  new Promise()

``` javascript
new Promise((resolve, reject) => {
    resolve(1)
  })
    .then(
      (value) => {
        console.log('value 1', value)
        return 'value1'
      },
      (reason) => {
        console.log('reason 1', reason)
        return Promise.reject('reason1')
      }
    )
    .then(
      (value) => {
        console.log('value 2', value)
        return new Promise((resolve, reject) => {
          setTimeout(() => {
            resolve('异步的value2')
          }, 1000)
        })
      },
      (reason) => {
        console.log('reason 2', reason)
      }
    )
    .then((value) => {
      console.log('value 3', value)
    })
```



### 6. promise异常传透

> *不写reason，相当于指定了默认：reason=> throw reason*

1. 当使用promise的then链式调用时，可以在最后指定失败的回调

   ``` javascript
   new Promise((resolve, reject) => {
       throw '开始就抛出了异常'
     })
       .then((value) => value) // 不写reason，相当于指定了默认：reason=> throw reason
       .then((value) => value)
       .then((value) => value)
       .catch((error) => {
         console.log('最后catch的接收', error)
       })
   ```

2. 前面任何操作出了异常，都会传到最后失败的回调中处理

   ``` javascript
   new Promise((resolve, reject) => {
       resolve(0)
     })
       .then((value) => value)
       .then((value) => {
         throw 'onResolved2 抛出了异常'
       })
       .then((value) => value)
       .catch((error) => {
         console.log('最后catch的接收', error)
       })
   ```

   

### 7. 中断promise链

> 当使用promise的then链式调用时，在中间中断，不再调用后面的回调函数

办法：在回调函数返回一个pending状态的promise对象

``` javascript
new Promise((resolve, reject) => {
    resolve(1)
  })
    .then((value) => {
      console.log('onRejected 1')
    })
    .then((value) => {
      return new Promise(() => {}) // 中断
    })
    .then((value) => value)
    .catch((error) => {
      console.log('catch', error)
    })
    .then((value) => {
      console.log('最后一个then ', value)
    })
}
```



