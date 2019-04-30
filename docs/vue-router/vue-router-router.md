## 谈谈 vue-router 中的 router

在vue组件中可以这样得到
```js
export default {
  name: 'TestTHird',
  mounted(){
    console.log(this.$router)
  }
}
```
### 1. Router实例属性

* router.app

配置了 router 的 Vue 根实例

* router.currentRoute
  * 类型： `route`
  当前路由对应的`路由信息对象`

### 2. 路由实例方法
* 导航守卫： `vue-router` 提供的导航守卫主要用来通过跳转或者取消的方式守卫导航。有很多机会植入路由导航过程中：`全局的`，`单个路由独享的`，或者 `组件级别的`
  1. 全局前置守卫：`router.beforeEach`

  ```js
  const router = new VueRouter({ ... })
  router.beforeEach((to, from, next) => {
    // ...
  })
  ```
  
  当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 `等待中`。
  * `to`和`from`都很好理解，我们重点来讲讲`next`
  导航守卫方法一定最后要调用 next()方法来resolve这个钩子，执行的效果依赖next()方法的调用参数

    * `next()`:进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed (确认的)
      注意:next() 表示路由成功，直接进入to路由，不会再次调用router.beforeEach()
      其他next('XXX')会再次调用router.beforeEach()


    * `next(false)`:中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 from 路由对应的地址

    * `next('/)`或者 `next({path:'/'})`: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。

    * `next(error)`:


  2. 全局解析守卫 :`router.beforeResolve`
    用`router.beforeRsolve`注册一个全局守卫，跟 `router.beforeEach`类似，区别是在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后**，解析守卫被调用

  3. 全局后置钩子: `router.afterEach`

  ```js
  router.afterEach((to, from) => {
    // 注册全局后置钩子，不接受`next`函数，也不会改变导航本身
  })
  ```
  
  该函数会在每次路由切换成功进入激活阶段时被调用。
  注意，该函数调用时仅仅意味着切换已经被验证过了，也就是所有 `canDeactivate` 和 `canActivate` 钩子函数都成功的被断定( `resolved` )了，而且浏览器地址栏中的地址也已经更新。并不能保证所有的 `activate` 钩子函数都被断定了。


* 路由独享的守卫,可以在路由配置上直接定义`beforeEnter`守卫

``` js
  const router = new VueRouter({
    routes: [
      {
        path: '/foo',
        component: Foo,
        beforeEnter: (to, from, next) => {
          // ...
        }
      }
    ]
  })
```

* 组件内的守卫: 就是在路由组件内直接定义以下路由导航守卫
  1. beforeRouteEnter
  2. beforeRouteUpdate
  3. beforeRouteLeave

```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```




