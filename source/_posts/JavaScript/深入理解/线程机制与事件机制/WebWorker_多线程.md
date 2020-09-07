> **H5规范提供了JS分线程的实现，`Web Workers`**

### `Web Workers`相关API

1. `Worker`：构造函数，加载分线程执行的JS文件（需额外的加载JS文件）
2. `Worker.prototype.onmessage` :接收另一个线程的回调函数
3. `Worker.prototype.postMessage`:想另一个线程发送消息

### `Web Workers`不足

1. `worker`内代码不能操作DOM（更新UI）
2. 不能跨域加载JS
3. 不是每个浏览器都支持([兼容性](https://caniuse.com/#search=Web Workers))

### 作用

可以将大量计算的代码交由WebWorker，避免主线程因为大量计算耗费时间而被阻塞