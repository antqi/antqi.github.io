## 隔一秒打印 i 值

### 方式一 - 闭包

```javascript
for (let i = 0; i < 5; i++) {
  ;(function(number) {
    setTimeout(function() {
      console.log(number)
    }, 1000 * number)
  })(i)
}
```

### 方式二 - ES6 箭头函数

```javascript
for (let i = 0; i < 5; i++) {
  setTimeout(() => {
    console.log(i)
  }, i * 1000)
}
```

### 方式三 - Promise

```javascript
function waitTime(i) {
  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      resolve(i)
    }, i * 1000)
  })
}

for (let i = 0; i < 5; i++) {
  waitTime(i).then(function(res) {
    console.log(res)
  })
}
```
