### 谈谈 vue.filter(id,[definition])
* 参数
  1. `{string} id`
  2. `{Function} [definition]`

* 用法：
  注册或获取全局过滤器

  ```js
  // 注册
  Vue.filter('my-filter', function (value) {
    // 返回处理后的值
  })
  // getter，返回已注册的过滤器
  var myFilter = Vue.filter('my-filter')

  ```
