# 闭包
最近面试老是被问闭包，好吧，我对闭包理解的不算透彻，今天打算彻底理解下闭包

- 何为闭包？<br />
`有权限访问另外一函数作用域中的变量的函数。`常见的创建闭包的方式，就是在一个函数内部创建另一个函数，然后return 出去。例如：

```js
function createComparisonFunction(propertyName) {
  return function(obj1, obj2) {
    var v1 = obj1[propertyName];
    var v2 = obj2[propertyName];
    if (v1 <  v2) {
      return -1
    } else if (v1 > v2) {
      return 1
    } else {
      return 0
    }
  }
}
```
在这个例子中，v1，v2 访问了外部函数中的变量propertyName。即使这个内部函数被返回了，而且在其他地方被使用了，但是它仍然可以访问变量propertyName。之所以能够访问这个变量，因为内部函数的作用域链中包含了createComparisonFunction 的作用域。<br />
我认为闭包要结合js 的作用域才能理解清楚。

- 作用域<br />
以下面的C 函数为例

```js
function C(v1,v2) {
  if (v1 <  v2) {
      return -1
  } else if (v1 > v2) {
      return 1
  } else {
      return 0
  } 
}
```
每次定义C函数，会创建一个属于这个C函数的[[scope]]属性，其只包括全局变量对象。
在执行的时候，为函数创建一个执行环境，执行环境创建了作用域链[[ scope chain ]],先把全局变量对象复制到作用域链中，然后把本地活动对象推到作用域链的顶端。所以函数在访问一个变量的时候，就会从作用域链的顶端开始找。一般来讲，当函数执行完毕后，本地活动对象会被销毁，内存中仅保存全局作用域（全局执行环境的变量对象）。<br />
但是闭包的情况又不一样

- 闭包的作用域链<br />
在另一个函数内部定义的函数会将包含函数(即外部函数)的活动对象添加到它的作用域链中。
```js
  var compare = createComparisonFunction('name');
  var result = compare({name: 'Nick'},{name: 'Greg'});
```
在匿名函数从createComparisonFunction返回后，它的作用域[[scope]]中包含createComparisonFunction()函数的活动对象和全局变量对象。这样匿名函数就可以访问在
中包含createComparisonFunction()中的定义的所有变量。当createComparisonFunction()执行完毕后，其执行环境的作用域链会被销毁，但是它的活动对象任然保存在内存中，直到compare被销毁，例如：
```js
  // 创建函数
  var compareNames = createComparisonFunction('name');
  // 调用函数
  var result = compare({name: 'Nick'},{name: 'Greg'});
  // 解除对匿名函数的引用（以便释放内存）
  compareNames = null ;
```
- 为什么会有闭包？
  `保证内部变量不被污染`
- 闭包的应用场景
  * [函数防抖和节流](./throttle.md)




