### 怎么给 vue  写一个插件
插件通常会为Vue添加全局功能,插件的范围没有限制-- 一般有下面的几种
* 添加全局方法或者属性
* 添加全局资源：指令/过滤器/过渡等，如 `vue-touch`
* 通过全局 mixin 方法添加一些组件选项, 如: vue-router
* 添加 Vue 实例方法,通过把它们添加到 Vue.prototype 上实现

#### 开发插件

Vue.js 的插件应该有一个公开方法 install。这个方法的第一个参数是 Vue 构造器，第二个参数是一个可选的选项对象：

```js
  MyPlugin.install = function (Vue, options) {
  // 1. 添加全局方法或属性
  Vue.myGlobalMethod = function () {
    // 逻辑...
  }
  // 2. 添加全局资源
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // 逻辑...
    }
    ...
  })
  // 3. 注入组件
  Vue.mixin({
    created: function () {
      // 逻辑...
    }
    ...
  })
  // 4. 添加实例方法
  Vue.prototype.$myMethod = function (methodOptions) {
    // 逻辑...
  }
}
```

#### 使用插件
```js
// 通过全局方法 Vue.use() 使用插件。它需要在你调用 new Vue() 启动应用之前完成
// 调用 `MyPlugin.install(Vue)`
Vue.use(MyPlugin,{options})
new Vue({
  //... options
})
```
#### 一个小例子

在plugins/test 中

```js
  // plugins/tes
  export default {
    install(Vue) {
      Vue.myGlobalMethod = function() {
        console.log('myGlobalMethod')
      }
      Vue.directive('my-directive', {
        bind(el, binding, vnode, oldVnode) {
          console.log('directive')
        }
      })
    
      Vue.mixin({
        beforeCreate() {
          console.log('beforeCreate')
        },
        created() {
          console.log('create')
        },
        mounted() {
          console.log('mounted')
        }
      }) // 一旦使用全局混入对象，将会影响到所有之后创建的Vue实例。
      Vue.prototype.$myMethod = function(methodOptions) {
        console.log(methodOptions)
      }
    }
  }

  //  在main.js 中
  import Test from '@/plugins/test'
  Vue.use(Element)
  Vue.use(Test)

```
然后你可以在组件中发现，因为设置了全局的mixin,给每个组件都增加了`beforeCreate` `created` `mounted` 生命周期,
并且在组件中可以用注册的方法 `$myMethod`

```js
methods: {
    change() {
      this.$myMethod('test')
      this.message = '父组件已更新'
    }
  }
}
```

