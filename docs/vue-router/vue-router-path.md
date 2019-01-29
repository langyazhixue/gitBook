## Vue Router path 中通配符
```js
import VueRouter from 'vue-router'
import Vue from 'vue'
Vue.use(VueRouter)
export const constantRouterMap = [
  {
    path:'/',
    redirect:'/login',
    component: () => import('')
  },
   {
    path:'/hello/:name', // 1. 匹配 /hello/car 2. 匹配 /hello/bus
    component: () => import('')
  },
  {
    path:'/hello（/:name）', // 1. 匹配 /hello/car 2. 匹配 /hello
    component: () => import('')
  },
  {
    path:'/files/*.*', // 1. 匹配 /files/cat.jpg 2. 匹配 /files/cat.html
    component: () => import('')
  },
  {
    path:'/files/*', // 1. 匹配 /files/ 2. 匹配 /files/a 3. 匹配 /files/a/b
    component: () => import('')
  },
  {
    path:'/**/*.jpg', // 1. 匹配 /files/cat.jpg 2. 匹配 /files/a/b/cat.jpg
    component: () => import('')
  },
  {
    path:'*',
    component:() => import('@/views/404')
  }
]
const router = new VueRouter({
  mode:"history", // 默认 hash, history  要跟服务器配合使用，慎用
  routes:constantRouterMap
})
export default router
```

`通配符的规则如下`
* :paramName

:paramName 匹配 URL的一个部分，直到遇到下一个/、？为止。这个路径参数可以通过 this.$route.params.paramName  取出

* ()

()表示URL的这个部分是可选的

* `*`

`*` 匹配任意字符，直到模式里面的下一个字符为止。匹配方式是非贪婪模式

* `**` 

* `**` 匹配任意字符，直到模式里面的下一个字符为止。匹配方式是贪婪模式

* `redirect`

`redirect` 重定向：有时候开发中我们虽然设置的路径不一致，但是有时候我们希望跳转到同一个页面，或者说是打开同一个组件，这时候需要`redirect`这个参数