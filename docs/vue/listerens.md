### `$attrs`和`$listeners`的使用

![图1](./img/listerens/1.jpg)

<font color="#006600">\#</font> **现在我们讨论下 A组件和 B 组件 如何通信，有几种解决方案**

1. **Vuex:**
  用Vuex来进行数据管理，但是如果项目中**多个组件共享状态比较少**，项目比较小，并且全局状态比较少，那使用`Vuex`来实现该功能，并没有发挥出`Vuex`威力

2. **通过B组件**
  使用B来做中转站，当**A组件**需要把信息传给**C组件**时候，B接受**A组件**的信息，然后利用属性传给**C组件**，这是一种解决方案，但是如果嵌套的组件过多，会导致代码繁琐，代码维护比较困难；如果**C中状态的改变需要传递给A，使用事件系统一级级往上传，**这样就非常麻烦

3. 自定义一个数据总线 `new Vue()`，这种情况碰到组件跨级传递消息，但是使用Vuex感觉有点浪费，但是缺点是碰到多人合作时候，代码的维护性较低，代码可读性低
&nbsp;
在很多开发情况下，我们只是想把**A组件**的信息传递给C组件，如果使用`props`绑定来进行信息的传递，虽然能够实现，但是代码并不美观
&nbsp;
所以在`Vue2.4` 中，作者 引入了 `$attrs`和 `$listeners`,新增了`inheritAttrs` 选项,关于 `$attrs`和  `inheritAttrs` 这里不说了，具体可以看[谈谈inheritAttrs](../vue-other/inheritAttrs.md),我们这里看看 **$listeners** 的使用

#### example

**父组件**
```js
<template>
  <div class="test-container">
   <listeners-child
    :items='items'
    @updateItems='receveNewItems'
   ></listeners-child> 
  </div>
</template>
<script>
import listenersChild from './listenersChild'
export default {
  name: 'TestTwo',
  components:{
    listenersChild
  },
  data() {
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
  methods:{
    receveNewItems(newItem){
      this.items.push(newItem)
    }
  }
}
</script>
```
**二层组件**

```html
<template>
  <div class="hello">
    <my-child
    v-bind ='$attrs'
    v-on="$listeners"
    />
  </div>
</template>
<script>
import MyChild from './child.vue'
export default {
  data () {
    return {
      num: 100
    }
  },
  components:{
    MyChild
  }
}
</script>
```

**三层组件**
```html
<template>
  <div class="hello">
    <div>
      <slot name='header'></slot>
    </div>
    <ul>
      <li
        v-for="todo in items"
        :key="todo.addr"
      >{{todo.text }}
      </li>
    </ul>
    <button @click='add'>add</button>
  </div>
</template>
<script>
export default {
  name:'MyChild',
  data () {
    return {
      num: 100
    }
  },
  props: ['items'],
  methods: {
    add() {
      this.$emit('updateItems',{
        text: '文字33', cname: 'hangzhou', addr: 'hangzhou'
      })
    }
  }
}
</script>
```