### vue-router Router 构件项

```js
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router)

export dafault new Router({
  routes: [
    RouteConfig1,
    RouteConfig2
  ],
  mode:'hash',
  linkActiveClass:'linkActiveClass',
  linkExactActiveClass:'linkExactActiveClass',
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition
    } else {
      const position = {}
      if (to.hash) { // 如果路径中有哈希值，就采用锚点定位 // 在history情况下
        position.selector = to.hash
      } else if (to.matched.some(m => m.meta.scrollToTop)) { // 如果路由元信息中存在参数，对参数做进一步判断（此示例代表滚动到顶部）
        position.x = 0
        position.y = 0
      }  else {
        position.x = 0
        position.y = 0
      }
      return position
    }
  },
  parseQuery(query){
    let res = {}
    if(query) {
      res = pathParameterToJson(query)
    }
    return res
  }
})
```

#### 1. routes

```js
RouteConfig = {
  path: string;
  component?: Component;
  name?: string; // 命名路由
  components?: { [name: string]: Component }; // 命名视图组件
  redirect?: string | Location | Function;
  props?: boolean | Object | Function;
  alias?: string | Array<string>;
  children?: Array<RouteConfig>; // 嵌套路由
  beforeEnter?: (to: Route, from: Route, next: Function) => void;
  meta?: any;

  // 2.6.0+
  caseSensitive?: boolean; // 匹配规则是否大小写敏感？(默认值：false)
  pathToRegexpOptions?: Object; // 编译正则的选项
}
```
* alias 别名

“重定向”的意思是，当用户访问 /a时，URL 将会被替换成 /b，然后匹配路由为 /b，那么`别名`又是什么呢？

`/a` 的别名 是`/b`,意味着，当用户访问 `/b`时候，URL会保持为`/b`,但是路由匹配为`/a`,就像用户访问`/a`一样

```js
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```
`别名` 的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。

* props
就是在路由中配置一个`props` 参数, 然后可以在组件实例中获得配置的props

1. 对象模式

```js
{
  path: '/search',
  component: () => import('@/views/login/index'),
  alias: '/search22',
  props: {
    query: 11
  }
}
```

2. 函数模式

```js
const router = new VueRouter({
  routes: [
    { path: '/search', component: SearchUser, props: (route) => ({ query: route.query.q }) }
  ]
})
```
在 组件中应用

```js
props: {
  query: {
    default: 0,
    type: Number
  }
}
```
#### 2. mode 指的是URL模式，可选值 `"hash" | "history" | "abstract"`
配置路由模式
1. `hash` :*默认模式*  使用 URL hash 值来作路由。支持所有浏览器，包括不支持 HTML5 History Api 的浏览器
2. `history`: 依赖 HTML5 History API 和服务器配置。查看 HTML5 History 模式。
3. `abstract`: 支持所有 JavaScript 运行环境，如 Node.js 服务器端。如果发现没有浏览器的 API，路由会自动强制进入这个模式。

#### 3. base 
1. 默认值：'/'
应用的基路径。例如，如果整个单页应用服务在 /app/ 下，然后 base 就应该设为 "/app/"

####  4. linkActiveClass

#### 5. linkExactActiveClass

#### 6. scrollBehavior
`scrollBehavior`选项可以帮助页面重新加载的时候想要页面滚动到顶部，或者是保持原先的滚动位置。

#### 7. parseQuery
提供自定义查询字符串的解析/反解析函数






