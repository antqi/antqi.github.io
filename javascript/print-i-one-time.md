## 隔一秒打印 i 值

### 方式一

```
for (let i = 0; i < 5; i++) {
  ;(function(number) {
    setTimeout(function() {
      console.log(number)
    }, 1000 * number)
  })(i)
}

```
