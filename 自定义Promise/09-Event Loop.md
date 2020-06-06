## Event Loop是什么

> event loop 是一个**执行模型**，在不同的地方有不同的实现。浏览器和NodeJs基于不同的技术实现了各自的Event Loop

- 浏览器的Event Loop是在[html5的规范](https://www.w3.org/TR/html5/webappapis.html#event-loops)中明确定义。但只是定义了浏览器中Event Loop的模型，具体实现留给了浏览器厂商

- NodeJS的Event Loop是基于libuv实现的。可以参考Node的[官方文档](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)以及libuv的[官方文档](http://docs.libuv.org/en/v1.x/design.html)。并且libuv对Event Loop做出了实现。



## 宏队列和微队列

**宏队列-macrotask，也叫tasks**。一些异步任务的回调回依次进入macro task queue，等待后续被调用。这些异步任务包括：

- setTimeout
- setInterval
- setImmediate(Node独有)
- requestAnimationFrame(浏览器独有)
- I/O
- UI rendering(浏览器独有)

**微队列-microtask，也叫jobs**。另一些异步任务的回调回依次进入micro task queue，等待后续被调用，这些异步任务包括：

- process.nextTick(Node独有)

- Promise

- Object.observe

- MutationObserver

  

## 浏览器的Event Loop

补充图

执行JavaScrept代码的具体流程：

1. 执行全局脚本同步代码，这些同步代码有一些事同步语句，有一些是异步语句（比如setTimeout）；
2. 全局脚本代码执行完毕后，调用栈Stack会清空；
3. 从任务队列中取出可执行任务，也就是微队列和宏队列，<u>优先微队列</u>；
4. 从微队列microtask queue中取出位于队首的回调任务，放入调用栈Stack中执行，执行完后microtask queue 长度-1
5. 继续取出位于队首的任务，放入调用栈Stack中执行，以此类推重复，直到microtask queue中的所有任务都执行完毕。如果在执行microtask的过程中，又产生了microtask，那么会加入到队列的末尾，也会在这个周期被调用执行；
6. microtask queue 中所有任务都执行完毕，此时microtask queue为空队列，调用栈Stack也为空；
7. 取出宏队列macrotask queue中位于队首的任务，放入Stack中执行；
8. 执行完毕后，调用栈Stack为空；
9. 重复步骤第3-8；

而以上，则是浏览器的Event Loop（事件循环）

归纳重点：

1. 宏队列macrotask一次只从队列中取一个任务执行，执行完后就去执行微队列中的任务；
2. 微任务队列中的任务会被一次取出执行，直到microtask queue为空；

## NodeJS中的Event Loop

### libuv 

![]( https://user-gold-cdn.xitu.io/2018/9/5/165a8667e0f09fa2?imageslim )

#### 执行宏队列的回调任务有6个阶段（按顺序）

1. timer阶段：执行`setTimeout` 和 `setInterval` 预定的`callbacks`；
2. I/Ocallbacks阶段：执行除了close事件的callbacks、被timers设定的callbacks、`setImmediate()`设定的callbacks之外的callbacks；
3. idle，prepare阶段：仅node内部使用；
4. poll阶段：获取新的I/O事件，适当的条件下node将阻塞在这里；
5. check阶段：执行`setInmmediate()`设定的callbacks；
6. close callbacks阶段：执行`sockets.on('close',...)`的callbacks

#### 4个主要宏队列（按顺序）

> 浏览器中只有一个宏队列。NodeJS中不同的macrotask会被放置在不同的宏队列中。

1. Timers Queue
2. I/O  Callbacks Queue
3. Check Queue
4. Close Callbacks Queue

#### 2个主要微队列（按顺序）

> 浏览器中只有一个微队列。NodeJS中不同microtask会被放置在不同的微队列中

1. Next Tick Queue ：放置process.nextTick(callback)的回调任务
2. Other Micro Queue：放置其他microtask，比如Promise

### Event Loop过程

1. 执行全局脚本的同步代码；
2. 执行microtask微任务，先执行所有Next Tick Queue中的所有任务，再执行Other Microtask Queue中的所有任务；
3. 开始执行macrotask宏任务，共6个阶段，从第一阶段开始执行相应的每一个阶段的macrotask中的所有任务。**是执行每个阶段的所有macrotask任务执行完毕后，再开始 step 2**；
4. Timer Queue --> step 2 --> I/O Queue --> step 2 --> Check Queue --> step 2 --> Close Callback Queue --> step 2 --> Timers Queue ......







