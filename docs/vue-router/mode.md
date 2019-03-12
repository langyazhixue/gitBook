### vue-router的hash模式和history的区别

### 1. 为什么要有hash 和 history 模式
对于 Vue 这类前端开发框架，为了构建`SPA`(单页面应用)，需要引入前端路由系统，这也就是 `Vue-Router`存在的意义。
前端路由的核心，就在于 —— `改变视图的同时不会向后端发出请求`

为了达到这个目的，浏览器当前提供了以下两种支持
*  `hash` —— 即地址栏 URL 中的 # 符号

比如这个 URL：`http://www.abc.com/#/hello`，hash 的值为 #/hello。它的特点在于：hash 虽然出现在 URL 中，“#”后面的内容不会被包括在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面

hash模式背后的原理是onhashchange事件,可以在window对象上监听这个事件

```js
window.onhashchange = function(event){
  console.log(event.oldURL, event.newURL);
  let hash = location.hash.slice(1);
}
```

关键的一点是 因为hash发生变化的url都会被浏览器记录下来，从而你会发现浏览器的前进后退都可以用了，这样一来，尽管浏览器没有请求服务器，但是页面状态和url一一关联起来


* `history` 

history 模式背后的原理利用了 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法。（需要特定浏览器支持）

```js

history.pushState({color:'red'}, 'red', 'red'})
window.onpopstate = function(event){  //监听
  console.log(event.state)
  if(event.state && event.state.color === 'red'){
      document.body.style.color = 'red';
  }
}
```
这两个方法应用于浏览器的历史记录栈，在当前已有的back,forward,go的基础上，提供了对历史记录信息修改的功能，只是当他们执行修改时候，虽然改变了当前的`URL`,但是浏览器不会立即向后端发送请求



### 1. 使用场景

一般情况下，hash和history都可以，除非你更在意颜值，`#`符号夹杂在`URL`里面看起来确实不太好

```hml
如果不想要很丑的 hash，我们可以用路由的 history 模式，这种模式充分利用 history.pushState API 来完成
URL 跳转而无须重新加载页面
```
另外：调用history.pushState()相比较于直接修改hash，存在以下优势：
* pushState() 设置新的 `URL` 可以是与当前 `URL` 同源的任意 `URL`；而 `hash` 只可修改 # 后面的部分，因此只能设置与当前 `URL` 同文档的 URL；
* pushState() 设置的新 URL 可以与当前 URL 一模一样，这样也会把记录添加到栈中；而 hash 设置的新值必须与原来不一样才会触发动作将记录添加到栈中；
* pushState() 通过 stateObject 参数可以添加任意类型的数据到记录中；而 hash 只可添加短字符串；
* pushState() 可额外设置 title 属性供后续使用。

当然，`history` 也不是样样都好。`SPA`虽然在浏览器里面游刃有余，但是真要通过`URL`向后端发起请求时候， 就会有差异
尤其是用户手动输入`URL`回车之后，或者重新刷新(重启)浏览器的时候

1. hash 模式下，仅 hash 符号之前的内容会被包含在请求中，如 `http://www.abc.com`，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 404 错误
2.  history 模式下，前端的 URL 必须和实际向后端发起请求的 URL 一致，如 `http://www.abc.com/book/id` 如果后端缺少对 `/book/id` 的路由处理，将返回 `404` 错误。`Vue-Router 官网里如此描述：“不过这种模式要玩好，还需要后台配置支持……所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。”` 服务器要配合设置