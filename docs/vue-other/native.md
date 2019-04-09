### vue中'. native'修饰符的使用

官方解释：你可能想在某个组件的根元素上监听一个原生事件。可以使用 `v-on` 的修饰符 `.native`

通俗点讲：就是在父组件中给子组件绑定一个原生的事件，就将子组件变成了普通的HTML标签，不加`.native`事件是无法触发的

```html
<template>
  <div>
    <my-component @click='alert'></my-component>
  </div>
</template>
<script>
export default {
  name:'nativeTest',
  components:{
    myComponent:{
      template:`testView`
    }
  },
  methods:{
    alert(){
      window.alert('test')
    }
  }
}
</script>
```
点击毫无反应

如果加上.native


```html
<template>
  <div>
    <my-component @click.native='alert'></my-component>
  </div>
</template>
<script>
export default {
  name:'nativeTest',
  components:{
    myComponent:{
      template:`testView`
    }
  },
  methods:{
    alert(){
      window.alert('test')
    }
  }
}
</script>
```

可以理解为该修饰符的作用就是把一个vue组件转化为一个普通的HTML标签，并且该修饰符对普通HTML标签是没有任何作用的。

