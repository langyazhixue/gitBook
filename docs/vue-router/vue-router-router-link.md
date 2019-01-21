## <router-link>

`<router-link>` 组件支持用户在具有路由功能的应用中 (点击) 导航。 通过 to 属性指定目标地址
默认渲染成带有正确链接的 `<a>` 标签，可以通过配置 tag 属性生成别的标签.。另外，当目标路由成功激活时，链接元素自动设置一个表示激活的 CSS 类名。

## <router-link> Props

*to* 
* 类型：`string | Location`
* `required`

表示目标路由的链接。当被点击后，内部会立刻把 to 的值传到 router.push()，所以这个值可以是一个字符串或者是描述目标位置的对象

```html
<!-- 字符串 -->
<router-link to="home">Home</router-link>
<!-- 渲染结果 -->
<a href="home">Home</a>

<!-- 使用 v-bind 的 JS 表达式 -->
<router-link v-bind:to="'home'">Home</router-link>

<!-- 不写 v-bind 也可以，就像绑定别的属性一样 -->
<router-link :to="'home'">Home</router-link>

<!-- 同上 -->
<router-link :to="{ path: 'home' }">Home</router-link>

<!-- 命名的路由 -->
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>

<!-- 带查询参数，下面的结果为 /register?plan=private -->
<router-link :to="{ path: 'register', query: { plan: 'private' }}">Register</router-link>
```
*replace*

* 类型：`boolean`
* 默认值： `false`

设置 replace属性的话，当点击时候，会调用router.replace(),而不是router.push(),于是导航后不会留下history记录
```html
<router-link :to="{ path: '/abc'}" replace></router-link>
```
*append*
* 类型：`boolean`
* 默认值：  `false`

设置`append`属性后，则在当前相对路径前添加基础路径。例如我们从`/a`导航到一个相对路径 `/b`,如果没有配置`append`,则路径为`/b`,
如果配置了，则为`/a/b`

```html
<router-link :to="{ path: 'relative/path'}" append></router-link>
```
*tag*
* 类型： `string`
* 默认值：`a`
有时候想要 `<router-link>` 渲染成某种标签，例如`<li>`,我们使用`tag` prop 类型制定何种标签，同时它还会监听点击，触发导航

```html
  <router-link to="/foo" tag="li">foo</router-link>
  <!-- 渲染结果 -->
  <li>foo</li>
```
*active-class*
* 类型：`string`
* 默认值：`router-link-active`

 设置 链接激活时使用的 CSS 类名。默认值可以通过路由的构造选项 linkActiveClass 来全局配置。

*exact*
* 类型：`boolean`
* 默认值:`false`

`是否激活`默认类名的依据是`inclusive match`(全包含匹配)如果当前的路径是 /a 开头的，那么 <router-link to="/a"> 也会被设置 CSS 类名。

按照这个规则，每个路由都会激活<router-link to="/">！想要链接使用 "exact 匹配模式"，则使用 exact 属性：

```html
<!-- 这个链接只会在地址为 / 的时候被激活 -->
<router-link to="/" exact>
```
*exact-active-class*
* 类型：`string`
* 默认值：`router-link-exact-active`

配置当链接被精确匹配到的时候应激活的class。注意默认值也可以是通过路由构造函数选项
`linkExactActiveClass` 进行全局配置


