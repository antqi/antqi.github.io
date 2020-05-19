## 1. 定义整体结构

``` javascript
/**
 * 自定义Promise函数模块 (ES5)
 */

;(function (params) {
  /**
   * Promise
   * @param {Function} excutor 执行器函数
   */
  function Promise(excutor) {}

  /**
   * Promise原型对象的then，并返回一个新的promise对象
   * @param {Function} onResolved 指定成功的回调函数
   * @param {Function} onRejected 指定失败的回调函数
   * @return {Promise} promise 新的promise对象
   */
  Promise.prototype.then = function (onResolved, onRejected) {}

  /**
   * Promise原型对象的catch，并返回一个新的promise对象
   * @param {Function} onRejected 指定失败的回调函数
   * @return {Promise} promise 新的promise对象
   */
  Promise.prototype.catch = function (onRejected) {}

  /**
   * Promise函数对象resolve,并返回一个指定成功的promise
   * @param {any} value 执行成功的值
   */
  Promise.resolve = function (value) {}

  /**
   * Promise函数对象reject,并返回一个指定失败的promise
   * @param {reason} reason 执行失败的数据
   */
  Promise.reject = function (reason) {}

  /**
   * Promise函数对象all,返回一个promise数组，当所有Promise成功的时才成功，否则失败
   */
  Promise.all = function (promises) {}

  /**
   * Promise函数对象race，返回一个promise数组，其结果状态由第一个完成的promise决定
   */
  Promise.race = function (promises) {}

  // 向外暴露
  params.Promise = Promise
})(window)

```



## 2. Promise的构造函数的实现

``` javascript
  function Promise(excutor) {
    
     this.STATUS = {  // 定义状态类型
      PENDING: 'pending',
      RESOLVED: 'resolved',
      REJECTED: 'rejected',
    }
    this.status = this.STATUS.PENDING // 指定promise对象的出事状态
    this.data = undefined // 给promise对象一个用于存储结果数据的属性
    this.callbacks = [] // 每个元素的结构：{ onResolved(){}, onRejected(){} }

    function resolve(value) {
    }
    
    function reject(reason) {
    }

    // 立即同步执行执行器
    try {
      excutor(resolve, reject)
    } catch (error) {
      // 执行器抛出异常，则为promise对象状态为rejected
      reject(error)
    }
  }
```





## 3. Promise.then()/catch()的实现



## 4. Promise.resolve()/reject()的实现

#### resolved

``` javascript
    function resolve(value) {
      // 状态只能改一次
      if (this.status !== this.STATUS.PENDING) {
        return
      }

      // 改状态为resolved
      this.status = this.STATUS.RESOLVED
      // 保存value数据
      this.data = value

      // 如果有待执行的callback函数, 立即异步执行指定回调函数onResolved
      if (this.callbacks.length) {
        setTimeout(() => {
          // 放入队列执行所有成功的回调
          this.callbacks.forEach((callbacksObj) => {
            callbacksObj.onResolved(value)
          })
        })
      }
    }
```

#### rejected

``` javascript
function reject(reason) {
      // 状态只能改一次
      if (this.status !== this.STATUS.PENDING) {
        return
      }

      // 改状态为rejected
      this.status = this.STATUS.REJECTED
      // 保存value数据
      this.data = reason

      // 如果有待执行的callback函数, 立即异步执行指定回调函数onRejected
      if (this.callbacks.length) {
        setTimeout(() => {
          // 放入队列执行所有失败的回调
          this.callbacks.forEach((callbacksObj) => {
            callbacksObj.onRejected(reason)
          })
        })
      }
    }
```



## 5. Promise.all()/race()的实现



## 6. Promise.resolveDelay()/rejectedDelay()



## 7. ES5 function 完整版本



## 8. ES6 class完整版



