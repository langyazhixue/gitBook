### 谈谈Vue中的函数式组件

<font color="#006600">#</font> Vue中有一种组件没有管理任何状态，也没有监听任何传递给它的状态，也没有生命周期方法，实际上，它只是一个接受一些Prop的函数。在这样场景下，我们可以将组将标记为`functional`,这意味着它为状态(没有`响应式数据`)，也没有实例(没有`this上下文`)

:palm_tree: demo

```js
  Vue.component('my-component', {
  functional: true,
  // Props 是可选的
  props: {
    // ...
  },
  // 为了弥补缺少的实例
  // 提供第二个参数作为上下文
  render: function (createElement, context) {
    // ...
  }
})

```
在2.5.0以及以上版本中，如果你使用了`单文件组件`,那么基于模版的函数式组件可以这样声明：

```html
<template functional>
</template>
```
组件需要的一切都是通过 context 参数传递，它是一个包括如下字段的对象：

* `props`: 提供所有prop的对象

* `children`: VNode子节点的数组

* `slots`: 一个函数，返回了包含所有插槽的对象

* `scopedSlots`: 一个暴露传入的作用域插槽的对象

* `data`: 传递给组件的整个`数据对象`,作为`createElement`的第二个参数传入组件

* `parent`: 对父组件的引用

* `listeners`: 一个包含了所有父组件为当前组件注册的事件监听器的对象

* `injections`: 如果使用了`inject`选项，则该对象包含了应当被注入的属性

因为函数式组件只是函数，所以渲染开销也低很多。然而，对持久化实例的缺乏也意味着函数式组件不会出现在 `Vue devtools` 的组件树里。

在作为包装组件时它们也非常有用。比如，当你需要做这些时：
* 程序化地多个组件中选择一个代为渲染

* 在将 `children`、`props`、`data`传递给子组件之前操作它们。

```js
var EmptyList = { /* ... */ }
var TableList = { /* ... */ }
var OrderedList = { /* ... */ }
var UnorderedList = { /* ... */ }
Vue.component('smart-list', {
  functional: true,
  props: {
    items: {
      type: Array,
      required: true
    },
    isOrdered: Boolean
  },
  render: function (createElement, context) {
    function appropriateListComponent () {
      var items = context.props.items
      if (items.length === 0)           return EmptyList
      if (typeof items[0] === 'object') return TableList
      if (context.props.isOrdered)      return OrderedList
      return UnorderedList
    }
    return createElement(
      appropriateListComponent(),
      context.data,
      context.children
    )
  }
})
```
<font color="#006600">##</font> 向子元素或子组件传递特性和事件

在普通组件中，没有被定义为 prop 的特性会自动添加到组件的根元素上，将已有的同名特性进行替换或与其进行智能合并。

然而函数式组件要求你显式定义该行为：

```js
Vue.component('my-functional-button', {
  functional: true,
  render: function (createElement, context) {
    // 完全透传任何特性、事件监听器、子节点等。
    return createElement('button', 
    context.data, context.children)
  }
})
```

如果你使用基于模板的函数式组件，那么你还需要手动添加特性和监听器。因为我们可以访问到其独立的上下文内容，所以我们可以使用 data.attrs 传递任何 HTML 特性，也可以使用 listeners (即 data.on 的别名) 传递任何事件监听器。

```js
<template functional>
  <button
    class="btn btn-primary"
    v-bind="data.attrs"
    v-on="listeners"
  >
    <slot/>
  </button>
</template>
```