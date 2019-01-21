
### vue 为什么组件的data要写成返回对象的函数
Vue  官方文档中 告诉我们组件中的data 要写成 返回对象的函数？那这是为什么呢？

原因：对象为引用类型时候，当重用组件时候，由于数据对象都指向同一个data对象，当在一个组件中修改data时候，其他重用的组件中的data会同时被修改
而使用返回对象的函数时候，由于每次返回的都是同一个(Object的实例)，引用地址不同，则不会出现问题。

举个例子
```js
// 1.对象方式（所有重用的实例中的data均为同一个对象）
  var data = {
    x: 1
  }
  var vm1 = {
    data: data
  }
  var vm2 = {
    data: data
  }
  vm1.data === vm2.data // true，指向同一个对象
  // 2.函数方式（所有重用的实例中的data均为同一个函数）
  var func = function () {
    return {
      x: 1
    }
  }
  var vm3 = {
    data: func
  }
  var vm4 = {
    data: func
  }
  vm3.data() === vm4.data() // false，指向不同对象

```
注意：vue 为什么组件的data写成返回对象的函数，这个函数方式中data都指向同一个函数，但这个函数每次的返回值都是一个新的对象，可以尝试如下（效果相同）
 
```js
  {x:1} === {x:1} // false
  new Object({x:1}) === new Object({x:1}) // false
```



