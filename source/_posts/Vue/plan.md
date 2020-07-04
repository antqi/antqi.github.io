目标：熟悉MVVM 基本实现原理

主要三个方面：

- 数据代理
- 模版解析
- 数据绑定

我需要多长时间合适？



路线：

1. 准备

   熟悉关键原生API

   1. [].slice.call()：转换数组
   2. node.nodeType
   3. Object.defineProperty(obj,propName,{})
   4. Object.keys()
   5. DocumentFragment
   6. obj.hasOwnProperty(propName):是否是自身属性

2. 数据代理

   1. 利用Object.defineProperty截止数据属性，所有数据操作都通过数据代理

3. 模版解析

   1. el中的所有子节点
   2. 这里涉及递归，diff等
   3. 以及如何解析模版
      - 模版中的指令
        - 事件
        - 其他指令
      - 模版中的文本节点绑定
      - ...

4. 数据绑定

   1. 更新了data中的某个属性，如何引起模版更新？
   2. 如何监听数据变动？
   3. 模版中的事件如何触发data 的改变？
   4. 四个重要的对象
      1. Observer
         - 做数据劫持
         - 给data中所有属性重新定义属性描述(get/set)
         - 如何和Watcher建立联系？
      2. Dep（Depend）
         1. Dep是什么？
         2. Dep做了什么事情？
         3. Dep在什么时候创建？
         4. Dep与其他关键的关系
      3. Compiler
         1. 解析模版
         2. 解析表达式
         3. 如何与Wathcer关联？
      4. Watcher
         1. Watcher是什么？监听数据？
         2. 如果做到监听数据，又通知变更呢？
         3. 监听数据的深度如何？
         4. 什么时候创建

5. MVVM原理图

   整体脉络- 自己熟练的输出

6. 双向数据绑定

   如何实现？目标：熟悉并手写