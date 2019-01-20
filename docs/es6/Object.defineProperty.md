
### 谈谈Object.defineProperty
* 举例

```js
var person = {}
Object.defineProperty(person,'name',{
  value: '张三'
})
console.log(person.name)
```
*参数*
  1. 第一个参数：要设置的目标对象（必填）
  2. 第二个参数：需要定义的属性或方法的名称（必填）
  3. 第三个参数：目标属性所拥有的特性。（descriptor）（必填
  注意： 三个参数都是必填项，重点介绍第三个参数 descriptor

*descriptor*
  1. value：属性的值
  2. writable：如果为false，属性的值就不能被重写, 只能为只读了
  3. configurable：总开关，一旦为false，就不能再设置他的（value，writable，configurable）
  4. enumerable：是否可枚举（是否能在for...in循环中遍历出来或在Object.keys中列举出来）
  5. get：一会细说
  6. set：一会细说
*descriptor 默认值*
回头看第一个例子

``` js
var person= {};
Object.defineProperty(person, "name", {
  value: '张三'
})
console.log(person.name); // 张三

```

但是第一次的时候 可以简单的理解为（暂时这样理解）它会默认帮我们把writable，configurable，enumerable都设上值，而且值还都是false。
也就是说，上面代码和下面是等价的的（仅限于第一次设置的时候。

```js
var person = {};
Object.defineProperty(person ,"name",{
  value: '张三',
  writable :false,
  enumerable: false,
  configurable: false
});
console.log(person.name); // 张三

```
*configurable*
总开关，第一次设置 false 之后，，第二次什么设置也不行了，比如说

```js
var person = {};
Object.defineProperty(person,"name",{
  configurable: false
});
Object.defineProperty(person,"name",{
  configurable: true
});
//error: Uncaught TypeError: Cannot redefine property: name
```
就会报错了。。
注意上面讲的默认值。。。如果第一次不设置它会怎样。。会帮你设置为false。。所以。。第二次。再设置他会怎样？。。对喽，，会报错

*writable*
如果设置为false,就变成之读

```js

var person = {}; 
Object.defineProperty(person, "name", { 
    　　value : '张三',
    　　writable : false 
　　}
);
console.log(person.name); // 打印 张三
person.name = '李四'; // 没有错误抛出（在严格模式下会抛出，即使之前已经有相同的值）
console.log(person.name); // 打印 张三， 赋值不起作用。

```
*enumerable*
属性特性enumerable定义了对象的属性是否可以在for...in循环和Object.keys()被枚举

```js

var person = {}; 
Object.defineProperty(person, "name", { 
    　　value : '张三',
    　　enumerable: true
　　}
);
console.log(Object.keys(person)); // 打印 ["name"]
```

如果将enumerable改为false, for...in 类似

```js
var person = {}; 
Object.defineProperty(person, "name", { 
    　　value : '张三',
    　　enumerable: false
　　}
);
console.log(Object.keys(person)); // 打印 []

```

set和get

`在 descriptor 中不能同时设置访问器（get 和 set）和 wriable 或 value，否则会错，就是说想用 get 和 set，就不能用 writable 或 value 中的任何一个`
```js
var person= {};
var temp = [];
Object.defineProperty(person, 'name', {
  set: function(newVal) {
    temp['name'] = newVal;
    console.log('为person设置新的姓名：' + newVal);
  },
  get: function() {
    var _name =  temp['name'] || '默认姓名';
    console.log('获取person的姓名：' +  _name);
    return _name;
  }
});
person.name = '张三';      //打印 获取person的姓名：张三
console.log(person.name)  //打印 获取person的姓名：张三(如果不设置name，这里会打印'默认姓名')
```

