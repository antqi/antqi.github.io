## this由来

**解析器在调用函数每次都会向函数内部传递一个隐含的参数**

- 这个隐含的参数就是this，this指向一个对象

- 这个对象我们称为函数执行的上下文对象

  

### 根据函数的调用方式的不同，this会指向不同的对象

> this具体指向什么，和函数调用形式有关系，与创建无关

1. 以函数的形式调用，this永远是window

   ``` javascript
   var name='window'
   
   function fun() {
     console.log(this.name)
   }
   
   var obj = {
     name:'obj',
     sayName:fun
   }
   
   fun() // window
   ```

2. 以方法的形式调用时，this指向调用调用方法的对象

   ``` javascript
   var name='window'
   
   function fun() {
     console.log(this.name)
   }
   
   var obj = {
     name:'obj',
     sayName:fun
   }
   
   obj.sayName() // obj
   ```

   