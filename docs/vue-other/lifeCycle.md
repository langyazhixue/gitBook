###  谈谈Vue的实例方法/生命周期
---

<font color="#006600">#</font> **vm.$mount([elementOrSelector])** 

* 返回值 vm -自身
* 用法：
  如果 Vue 实例在实例化时没有收到 el 选项，则它处于“未挂载”状态，没有关联的 DOM 元素。可以使用 vm.$mount() 手动地挂载一个未挂载的实例
  &ensp;
  如果没有提供 elementOrSelector 参数，模板将被渲染为文档之外的的元素，并且你必须使用原生 DOM API 把它插入文档中。

  &ensp;

  这个方法返回实例自身，因而可以链式调用其它实例方法。

  ```js
  var MyComponent = Vue.extend({
  template: '<div>Hello!</div>'
  })

  //创建并挂载到 #app (会替换 #app)
  new MyComponent().$mount('#app')

  同上//
  new MyComponent({ el: '#app' })

  // 或者，在文档之外渲染并且随后挂载
  var component = new MyComponent().$mount()
  document.getElementById('app').appendChild(component.$el)  
  //  vue.js?e503:634 [Vue warn]: Failed to mount component: template or render function not defined.

  ```
&ensp;
<font color="#006600">#</font> **vm.$forceUpdate()** 

* 迫使 Vue 实例重新渲染。注意它仅仅影响实例本身和插入插槽内容的子组件，而不是所有子组件。

** :tada: 注意 ** 
> 如果你发现你需要在Vue中做一次强制更新，99.9%的情况，或者你可能依赖来一个未被Vue的响应式系统追踪的状态

&ensp;
<font color="#006600">#</font> **vm.$destory** 

* 用法： 完全销毁一个实例。清理它与其他实例的链接，解绑它的全部指令以及事件监听器。
  触发 `beforeDestroy` 和 `destory` 的钩子

> 在大多数场景中你不应该调用这个方法。最好使用 v-if 和 v-for 指令以数据驱动的方式控制子组件的生命周期。