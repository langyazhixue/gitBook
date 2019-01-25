### Vue中的provide/inject
我们在实际业务中，有些复杂的组件，里面有很多子组件，而且子组件嵌套的很深。然后我们需要从父组件传递一个参数到很深的子组件。
这个时候我们有几个方案

* 通过`props`一层层的往下传 (不喜欢这种方案)
*  把需要的值存在`vuex` 中 （还行吧）
* `provide/inject`（挺好的）

provider/inject：简单的来说就是在父组件中通过provider来提供变量，然后在子组件中通过inject来注入变量。
需要注意的是这里不论子组件有多深，只要调用了inject那么就可以注入provider中的数据。而不是局限于只能从当前父组件的prop属性来获取数据

*我们来看一个例子*

```html
<template>
  <div>
    <child-one></child-one>
  </div>
</template>
<script>
import ChildOne from './components/childone'
export default {
  name:'parent',
  provide:{
    for:'demo'
  },
  components:{
    ChildOne
  }
}
</script>

```
在这里我们在父组件中 provide  `for` 

在ChildOne 中定义一个子组件

```html
<template>
  <div class='one'>
      {{demo}}
    <child-two></child-two>
  </div>
</template>
<script>
import ChildTwo from './childtwo.vue'
export default {
  name:'ChildOne',
  inject:['for'],
  data(){
    return {
      demo:this.for
    }
  },
  components:{
    ChildTwo
  }
}
</script>
```

childTwo 定义另一个子组件
```html
<template>
  <div class='two'>
      {{demo}}
  </div>
</template>
<script>
export default {
  name:'ChildTwo',
  inject:['for'],
  data(){
    return {
      demo:this.for
    }
  }
}
</script>

```

我们在2个子组件中使用`inject`注入了provide提供的变量for,并将它提供了data属性

运行效果
![图1](./img/provide/1.jpg)

在上面例子中，只要在父组件中调用了，那么在这个父组件生效的生命周期内，所有的子组件都可以调用inject来注入父组件中的值。
