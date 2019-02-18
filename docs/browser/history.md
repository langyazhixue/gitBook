## history

History 对象包含用户（在浏览器窗口中）访问过的 URL。
History 对象是 window 对象的一部分，可通过 window.history 属性对其进行访问。


###  属性
* length 返回浏览器历史列表中的 URL 数量。

### 方法

History 对象最初设计来表示窗口的浏览历史。但出于隐私方面的原因，History 对象不再允许脚本访问已经访问过的实际 URL。唯一保持使用的功能只有 back()、forward() 和 go() 方法。

*  back() 加载 history 列表中的前一个URL
*  forward() 加载 history 列表中的下一个 URL。
*  go(number|URL) 加载 history 列表中的某个具体页面。 其中num大于0，则前进；小于0，则后退

### h5 扩展方法

*pushState(stateData, title, url)* 添加一条历史记录，不刷新页面

```js 
  stateData// 一个指定网址相关的状态对象,popstate事件触发时，该对象会传入回调函数中。如果不需要这个对象，此处可以填写null
  title //  新页面标题，但是所有浏览器目前都忽略这个值，因此可以填写null
  url // 新的网址，此处必须与前页面处在同个域名
```
*replaceState(stateData, title, url)*

*HTML5还新增了可以监听history和hash访问变化的全局方法*
*window.onpopstate*：当调用history.go()、history.back()、history.forward()时触发；pushState()\replaceState()方法不触发。
*window.onhashchange*：当前 URL 的锚部分(以 '#' 号为开始) 发生改变时触发。触发的情况如下:

1. 通过设置Location 对象 的 location.hash 或 location.href 属性修改锚部分；
2. 使用不同history操作方法到带hash的页面;
3. 点击链接跳转到锚点。
  1. 通过location.href创建一个新的url记录，其将当前url之后的记录清空，并在其后新增url；history长度由也改变了；history的每次新增操作，都会将其后记录清空，在其后新增记录。
  2. 过pushState方法创建一个新的url记录，其也是清空、再新增记录；
  3. 通过replaceState方法修改一个url记录，其不会产生新记录，而是将当前记录进行修改。

  *值得注意的是，通过pushState新增或者修改的history记录，被访问时，当前页面不刷新。而locaiton.href生成的记录被访问时，页面将进行刷新。*

###  vue router的本质
vue router 其实本质是在HTML5 historyAPI、window.onpopstate、window.onhashchange上做的封装，通过一定的规则映射到对应的方法上，通过监听变化，从而实现单页路路由
[链接](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/onpopstate)






