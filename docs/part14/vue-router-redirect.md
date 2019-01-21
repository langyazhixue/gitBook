## 谈谈vue-router中的redirect
业务场景：
* 我们一般在进入一个项目的时候，通常都会有一个首页，
* 有时候设置的路径不一致，但是我们希望跳转到同一个页面，或者说是打开同一个组件。这时候我们就用到了路由的重新定向redirect参数。

### 
* 一般情况

```js
import VueRouter from 'vue-router'
import Vue from 'vue'
Vue.use(VueRouter)
const constantRouterMap = [
  {
    path:'',
    redirect:'/login',
  },
  {
    path:'/login',
    component: ()=> import('@/views/login/index')
  },
  {
    path:'*',
    component: ()=> import('@/views/404/index'),
    name:'errPage'
  }
]
const router = new VueRouter({
  //mode: "history",
  routes: constantRouterMap
})
export default router

```

注意：重新定向的路径下必须有组件，不然v-router 会映射找到 errPage 组件

*  重定向时传递参数

vue也已经为我们设置好重定向时传递参数，我们只需要在ridirect后边的参数里复制重定向路径的path参数就可以了

```js
import VueRouter from 'vue-router'
import Vue from 'vue'
Vue.use(VueRouter)
const constantRouterMap = [
  {
    path:'',
    redirect:'/login',
  },
  {
    path:'/login',
    component: ()=> import('@/views/login/index')
  },
  {
    path:'/news/:newsId(\\d+)/:newsTitle',
    name:'news',
    component:()=> import('@/views/news/index')
  },{
    //  带参数的路由重定向
    path:'/gonews/:newsId(\\d+)/:newsTitle',
    redirect:'/news/:newsId(\\d+)/:newsTitle'
  },
  {
    path:'*',
    component: ()=> import('@/views/404/index'),
    name:'errPage'
  }
]
const router = new VueRouter({
  //mode: "history",
  routes: constantRouterMap
})
export default router

```

```html
<template>
  <div class='parent'>
    <header>父组件</header>
    <test-child :items='items' :todos='todos'>
      <template slot-scope="slotProps">
      <span>{{slotProps.todo.text}}</span>
    </template>
    </test-child>
    <div>
      <!-- name：命命路由 -->
      <router-link
        :to="{
          name:'news',
          params:{
            newsId:'1',
            newsTitle:'my'
          }
        }"
      >
        跳到新闻页面
      </router-link>
    </div>
  </div>
</template>
```

* tips：v-router 中还有一个命名路由，非常好用。有时候，通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。你可以在创建 Router 实例的时候，在 routes 配置中给某个路由设置名称。

```js
const router =  new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```
现在要链接到一个命名路由，可以给router-link 的 to 属性传递一个对象

```html
<router-link :to="{name: 'user',params:{ userId:123 }}"></router-link>
```
这跟代码调用 router.push()是一回事情：

```js
  router.push({name:'user',params: { userId: 123 }})
```
这两种方式都会把路由导航到/user/123 路径