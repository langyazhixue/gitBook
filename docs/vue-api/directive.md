### 谈谈 vue.directive(id,[definition])
* 参数
  1. `{string} id`
  2. `{Function | Object} [definition]`

* 用法
  注册或获取全局指令

```js
// 注册
Vue.directive('my-directive', {
  bind: function () {},
  inserted: function () {},
  update: function () {},
  componentUpdated: function () {},
  unbind: function () {}
})
// 注册 (指令函数)
Vue.directive('my-directive', function () {
  // 这里将会被 `bind` 和 `update` 调用
})
// getter，返回已注册的指令
var myDirective = Vue.directive('my-directive')
```



