### 谈谈 vue.component(id,[definition])
* 参数
  1. `{string} id`
  2. `{}Function} [definition]`

* 用法：
注册或者获取全局组件，注册还会自动使用给定的`id`设置组件的名称

```js
  // 注册组件，传入一个扩展过的构造器
  Vue.component('my-component', Vue.extend({ /* ... */ }))

  // 注册组件，传入一个选项对象 (自动调用 Vue.extend)
  Vue.component('my-component', { /* ... */ })

  // 获取注册的组件 (始终返回构造器)
  var MyComponent = Vue.component('my-component')

```