### this是什么？

- 任何函数本质上都是通过某个对象来调用,未直接指定则是window
- 所有函数内部都有一个变量`this`
- 它的值是调用函数的当前对象

### 如何确定this的值

- test() ：window

- p.test() ：p 

- new Test() :新创建的对象

- p.call(obj)  : obj

  

``` javascript
function Person(age){
  console.log(this)
  this.age=age
  
  this.getAge=function(){
    console.log(this)
    return this.age
  }
  
  this.setAge=function(age){
    console.log(this)
    this.age=age
  }
}

Person(10) // window
var p1=new Person(11) // p1
p1.getAge() // p1
var obj={age:20}
p1.getAge.call(obj) // obj

var test=p1.setAge
test(18) // window

function fun1(){
  function fun2(){
    console.log(this)
  }
  fun2()
}

fun1() // window
```



### QA

1. `new` 做了什么？