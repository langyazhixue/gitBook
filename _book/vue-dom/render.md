### render

* 类型： `(createElement: () => VNode) => VNode`
* 详细： 

字符串模板的代替方案，允许你发挥 JavaScript 最大的编程能力。该渲染函数接收一个 createElement 方法作为第一个参数用来创建 VNode。

```js
import Vue from 'vue'
import App from './App'
new Vue({
  el: '#app',
  render: h => h(App)
})
// h 是一个 createElement方法
```

> Vue 选项中的 render 函数若存在，则 Vue 构造函数不会从 template 选项或通过 el 选项指定的挂载元素中提取出的 HTML 模板编译渲染函数

