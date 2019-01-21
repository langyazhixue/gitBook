Vue 官方文档中关于插槽的文档说明很短，语言很精练，这篇文章我们来好好理理关于插槽的事情。<br/>
插槽其实是一块模板，对于任何一个组件，从模板种类的角度来分，可以分为非插槽模板和插槽模板。<br/>
非插槽模板指的是html模板，指的是‘div、span、ul、table'这些，非插槽模板的显示与隐藏以及怎样显示由插件自身控制；<br/>
插槽模板是slot，它是一个空壳子，因为它显示与隐藏以及最后用什么样的html模板显示由父组件控制。<br/>
但是插槽显示的位置确由子组件自身决定，slot写在组件template的哪块，父组件传过来的模板将来就显示在哪块。

## 单个插槽
首先是单个插槽，其实也可以成为默认插槽或者匿名插槽，是与具名插槽相对的，因为其没有name 的属性<br/>
单个插槽可以放置在组件的任意位置，但是就像它的名字一样，一个组件中只能有一个该类插槽。相对应的，具名插槽就可以有很多个，只要名字（slot=name属性）不同就可以了。

父组件
```js
<template>
 <div class="father">
  <h3>这里是父组件</h3>
  <child>
   <div class="tmpl">
    <span>菜单1</span>
   </div>
  </child>
 </div>
</template>
```
子组件

```js
<template>
 <div class="child">
  <h3>这里是子组件</h3>
  <slot></slot>  // 这块插槽的模板是<child></child>里面的内容
 </div>
</template>
```

因为父组件在<child></child>里面写了html模板，那么子组件的匿名插槽这块模板就是下面这样。也就是说，子组件的匿名插槽被使用了，是被下面这块模板使用了。

```js
<div class="tmpl">
  <span>菜单1</span>
</div>

```
## 具名插槽
具名插槽就是插槽加了name 属性。具名插槽可以在一个组件中出现N次，它可以出现在不同的位置。
父组件
```js
<template>
 <div class="father">
 <h3>这里是父组件</h3>
 <child>
  <div class="tmpl" slot="up">
    <span>菜单1</span>
  </div>
  <div class="tmpl" slot="down">
    <span>菜单-1</span>
  </div>
  <div class="tmpl">
    <span>菜单->1</span>
  </div>
 </child>
 </div>
</template>
```
子组件
```js
<template>
 <div class="child">
 // 具名插槽
 <slot name="up"></slot>
 <h3>这里是子组件</h3>
 // 具名插槽
 <slot name="down"></slot>
 // 匿名插槽
 <slot></slot>
 </div>
</template>
```

可以看到，父组件通过html模板上的slot属性关联具名插槽。没有slot属性的html模板默认关联匿名插槽。

## 作用域插槽 | 带数据的插槽 (slot-scope)
- 作用域插槽是一种特殊类型的插槽，用作一个(能被传递数据的)可重用模板，来代替已经渲染好的元素。
- 作用域插槽就是可以将子组件的值传到父组件供使用
- slot-scope 一个父组件只能有一类该插槽
-  子组件中`<slot :todo=todo>{{todo.text}}</slot>` 属于回退的内容，当父组件没有提供插槽的时候，可以作为默认的内容
- slot-scope 在Vue版本 2.5.0+ 后，不在限制在 `<template>` 元素上使用，而可以用在插槽内的任何元素或者组件上
- 结构 slot-scope 例如

```js
<test-slot v-bind:todos="todos" v-bind:items="items">
  <template slot-scope="{ todo }">
    <span>{{ todo }}</span>
  </template>
</test-slot>
```

父组件

```js
<template>
  <div id="app1">
    <h2>app</h2>
     <test-slot v-bind:todos="todos" v-bind:items="items">
      <!-- 将 `slotProps` 定义为插槽作用域的名字 -->
      <template slot-scope="slotProps">
        <span>{{slotProps.todo.text}}</span>
      </template>
    </test-slot>
  </div>
</template>
import testSlot from './child.vue'
export default {
  data () {
    return {
      items: [
        { text: '文字1', cname: 'tom', addr: 'usa' },
        { text: '文字2', cname: 'wangwu', addr: 'uk' },
        { text: '文字3', cname: 'zhangsan', addr: 'un' }
      ],
      todos: [
        { text: '文字1', isComplete: false, id: 1 },
        { text: '文字2', isComplete: true, id: 2 }
      ]
    }
  },
  methods: {

  },
  components: {
    testSlot
  }
}
```
子组件
```js
<template>
  <div class="hello">
    <ul>
      <li
        v-for="todo in todos"
        v-bind:key="todo.id"
      >
        <!-- 我们为每个 todo 准备了一个插槽，-->
        <!-- 将 `todo` 对象作为一个插槽的 prop 传入。-->
        <slot v-bind:todo="todo" v-bind:item="item">
          <!-- 回退的内容 -->
          {{ todo.text }}
        </slot>
      </li>
    </ul>
  </div>
</template>

export default {
  data () {
    return {
      num: 100
    }
  },
  props: ['items', 'todos'],
  methods: {
  },
  created () {
    console.log('todos', this.$props.todos)
  }
}
```

