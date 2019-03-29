### 实例属性
---

<font color="#006600">\#</font> **vm.$data**

* **类型：**`Object`
* **详细：**
  Vue 实例观察的数据对象。Vue实例代理了对其data对象属性的访问


<font color="#006600">\#</font> **vm.$props**

* **类型：** `Object`

<font color="#006600">\#</font> **vm.$el**

* **类型：** `Element`
* **只读**
* **详细：** 
Vue 实例使用的根 DOM 元素。

<font color="#006600">\#</font> **vm.$options**

* **类型：** `Object`
* **只读**
* **详细：**
  用于当前 Vue 实例的初始化选项。需要在选项中包含自定义属性时会有用处：
```js
  new Vue({
    customOption: 'foo',
    created: function () {
      console.log(this.$options.customOption) // => 'foo'
    }
  })
```

<font color="#006600">\#</font> **vm.$parent**

* **类型：** `Vue instance`
* **只读**
* **详细：** 
  父实例，如果当前实例有的话。


<font color="#006600">\#</font> **vm.$root**

* **类型：** `Vue instance`
* **只读**
* **详细：** 
  当前组件树的根 Vue 实例。如果当前实例没有父实例，此实例将会是其自己。

<font color="#006600">\#</font> **vm.$children**

* **类型：** `Array<Vue instance>`
* **只读**
* **详细：** 
  当前实例的直接子组件。需要注意 $children 并不保证顺序，也不是响应式的。如果你发现自己正在尝试使用 $children 来进行数据绑定，考虑使用一个数组配合 v-for 来生成子组件，并且使用 Array 作为真正的来源。

<font color="#006600">\#</font> **vm.$slot**

* **类型：** `{ [name:string]: ? Array<VNode>}`
* **只读**
* **详细：** 
  用来访问被<font color="#006600">插槽分发</font>。每个<font color="#006600">具名擦槽</font>有其相应的属性(例如：`v-slot:foo`中的内容将会在`vm.$slots.foo`中被找到)。 `default` 属性包括了所有没有被包含在具名插槽中的节点或`v-slot:default` 的内容。

  &nbsp; 
  在使用<font color="#006600">渲染函数</font>书写,访问 `vm.$slots`最有帮助。

  ```js
  Vue.component('blog-post', {
    render: function (createElement) {
      var header = this.$slots.header
      var body   = this.$slots.default
      var footer = this.$slots.footer
      return createElement('div', [
        createElement('header', header),
        createElement('main', body),
        createElement('footer', footer)
      ])
    }
  })
  ```

<font color="#006600">\#</font> **vm.$scopedSlots**

* **类型：** `{ [name: string]: props => Array<VNode> | undefined }`
* **只读**
* **详细：**
  用来访问<font color="#006600">作用域插槽</font>。对于包括`默认slot` 在内的每一个插槽，该对象都包含一个返回响应VNODE的函数
  &nbsp;
  `vm.$scopedSlots`在使用<font color="#006600">渲染函数</font>开发一个组件时候特别有效
  &nbsp;
  **注意：** 
    1. 作用域插槽函数现在保证返回一个 VNode 数组，除非在返回值无效的情况下返回 undefined。
    2. 所有的 `$slots` 现在都会作为函数暴露在 `$scopedSlots`中。


<font color="#006600">\#</font> **vm.$refs**

* **类型：** `Object`
* **只读**
* **详细：**
  一个对象，持有注册过 `ref` <font color="#006600">特性</font> 的所有 DOM 元素和组件实例

<font color="#006600">\#</font> **vm.$attrs**

:tada: <font color="#006600" size=4>|</font> &ensp; 2.4.0 新增
* **类型：** `{[key: string]: string}`
* **只读**
* **详细：**
  包含了父作用域中不作为prop被识别(且获取)的特性绑定(`class` 和 `style` 除外)。当一个组件没有声明任何`prop`时，这里会包含所有父作用域的绑定(`class`和 `style`除外)，并且可以通过`v-bind="$attrs`

<font color="#006600">\#</font> **vm.$listeners**

:tada: <font color="#006600" size=4>|</font> &ensp; 2.4.0 新增

* **类型：** `{ [key: string]: Function | Array<Function> }`
* **只读**
* **详细：**
包含了父作用域中的 (不含 .native 修饰器的) `v-on` 事件监听器。它可以通过 `v-on="$listeners"` 传入内部组件——在创建更高层次的组件时非常有用。具体用法可以看看[谈谈$attrs和$listeners的使用](../vue/listerens.md) 这篇文章


 

