## 关于Location对象

Location 对象包含有关当前 URL 的信息。 Location 对象是 Window 对象的一个部分，可通过 window.location 属性来访问。

###  获取URL方式包括
```js
  location.href
  location.toString()
  location.toLocaleString()
  document.URL
```
* `toString` 跟 `toLocalString` 比较

两种方法的作用都是将传入的值转化为字符串类型，区别如下

1. 当被转化的值是个时间对象时，toLocaleString会将转化的结果以本地表示
```js
(new Date).toString();
 // "Mon Nov 06 2017 13:02:46 GMT+0800 (China Standard Time)"
(new Date).toLocaleString();
//"2017/11/6 下午1:03:12"
```

2. 另外当被转化的值是个时间戳时，toLocaleString会把时间戳每三位添加一个逗号，代码如下。
```js
(Date.parse(new Date())).toLocaleString()
//"1,509,944,637,000"
(Date.parse(new Date())).toString()
// "1509944643000"
// Date.parse()函数的返回值为Number类型，返回该字符串所表示的日期与 1970 年 1 月 1 日 0时0分0秒之间相差的毫秒数。
// getTime 方法的返回值一个数值，表示从1970年1月1日0时0分0秒（UTC，即协调世界时）距离该日期对象所代表时间的毫秒数。
```


### 设置URL 的方式

```html
location.href = " xxx"; //不加协议会默认为相对路径，location="xxx"类似
```

### location对象的其他属性包括

```js
// 假设当前的 URL 是: http://example.com:1234/test.htm?name=2#part2：
location.href  // 完整的URL
// http://example.com:1234/test.htm?name=2#part2
location.host           //主机加端口号
// example.com:1234
location.hostname       //主机
// example.com
location.port           //端口号
// 1234
location.protocol       //协议
// http:
location.pathname       //路径
// /test.htm?name=2#part2
location.hash           //片段标识符，可以用于保存网页状态
// #part2
// 假设设置了Location对象的 hash 属性，那么浏览器就会转移到当前文档中的一个指定的位置。
location.search         //返回问号后的字段
// ?name=2
```
以上属性都是可写的

### 该对象还有其他方法

```js

location.assign() // 加载新的文档 会在浏览器的历史记录中增加一条新纪录;
location.replace() // 会使用新URL覆盖浏览器的当前历史记录
location.reload()  // 会重新加载该页面

```
### 衍生知识点
html 中锚点的应用，在比较长的页面中锚点的使用会增加用户体验，比如说前程无忧用户的简历就用了锚点
* 在同一个页面中

```html
<a name='add'></a> <!-- 定义锚点 -->
<a href="#add">跳转到add</a>
```
* 在不同页面中，锚点定位在a.html中，从另外一个页面的链接跳转到这个锚点

 ``` html 
 <a href="a.html#add">跳转到a.add</a>

 ```
 * 点击链接触发js事件，同时跳转到锚点
  1. 
  ```html
    <a href="#add" onclick="add()">触发add函数并跳转到add锚点</a>
  ```
  2. 
  ``` html
  <p id="pNode"><!-- contents --></p><!-- 假设一个需要跳转到的节点 -->
  <a href="#" onclick="document.getElemetnById('pNode').scrollIntoView(true);return false;">通过scrollIntoView实现锚点效果</a>
  ```
  
