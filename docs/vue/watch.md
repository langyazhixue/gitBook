###  谈谈Vue中的watch
Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：侦听属性--`watch`,
`watch`可以监听组件的`props`中的属性、`data`中的属性、`computed`中的属性。
watch 的用法大致有三种

#### 1. 最简单的用法
```js
<template>
  <div>
    <input type="text" v-model="cityName">
  </div>
</template>
<script>
import _ from 'lodash'
export default {
  name:'watch',
  data(){
    return {
      cityName:'sjamgjao'
    }
  },
  created: function () {
    // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
    // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
    // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
    // 请参考：https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods:{
    getAnswer(){
      console.log(this.cityName)
    }
  }, 
  watch:{
    cityName(newVal,oldName){
      this.debouncedGetAnswer()
    }
  }
}
</script>

```
这种方式是直接写一个监听处理函数，当每次监听到cityName 值发生变化时候，执行函数

#### 2.  immediate

这样使用watch时有一个特点，就是当值第一次绑定的时候，不会执行监听函数，只有值发生改变才会执行。如果我们需要在最初绑定值的时候也执行函数，则就需要用到immediate属性。这个时候不能调用 methods中的方法

```js
  watch:{
    cityName:{
      handler(newVal,oldName){
        console.log(newVal)
        // this.debouncedGetAnswer() // Error in callback for immediate watcher "cityName": "TypeError: this.debouncedGetAnswer is not a function"
      },
      immediate: true
    }
  }
```

#### 3. deep
 watch 中还有一个属性deep，默认值是false，比如我们 data 里有一个obj属性：
  ```html
  <template>
    <div>
      <input type="text" v-model="obj.cityName">
      <input type="text" v-model="obj.address">
    </div>
  </template>
  <script>
  import _ from 'lodash'
  export default {
    name:'watch',
    data(){
      return {
        obj:{
          cityName:'sjamgjao',
          address:'杭州'
        }
      }
    },
    methods:{
      getAnswer(){
        console.log(this.cityName)
      }
    },  
    watch:{
      obj:{
        handler(newVal,oldName){
          console.log(newVal)
        },
       deep:false
      }
    }
  }
  </script>

  ```

  * 我们在在输入框中输入数据视图改变`cityName`或者`address`时候，我们可以发现`handler`方法并不执行。
  当我们在在输入框中输入数据视图改变`obj.cityName`的值时，我们发现是无效的。受现代 JavaScript 的限制 (以及废弃 Object.observe)，
  Vue 不能检测到对象属性的添加或删除。由于 Vue 会在初始化实例时对属性执行 `getter/setter` 转化过程，所以属性必须在 data 对象上存在才能让 Vue 转换它，这样才能让它是响应的。

  * 默认情况下 handler 只监听obj这个属性它的引用的变化，我们只有给obj赋值的时候它才会监听到，比如我们在 mounted事件钩子函数中对obj进行重新赋值
  ```js
    mounted: {
      this.obj = {
        cityName:'ningbo',
        address:'宁波'
     }
  ```
  * 这样我们的handler才会执行，那怎么样才能监听`obj`中的属性`cityName` 或者 `address`呢？这个时候deep就上场了。
  deep的意思就是深入观察，监听器会一层层的往下遍历，给对象的所有属性都加上这个监听器，但是这样性能开销就会非常大了，任何修改obj里面任何一个属性都会触发这个监听器里的 handler
  * 如果我们只想监听一个字符串呢 `cityName`
  ```js
    watch:{
      'obj.cityName':{
        handler(newVal,oldVal){
          console.log(newVal)
        },
        deep:true
      }
    }
  ```
  * 注意：watch可以进行多层深层次的监听

#### 4.注销watch
  为什么要注销 watch？因为我们的组件是经常要被销毁的，比如我们跳一个路由，从一个页面跳到另外一个页面，那么原来的页面的 watch 其实就没用了，这时候我们应该注销掉原来页面的 watch 的，不然的话可能会导致内置溢出。好在我们平时 watch 都是写在组件的选项中的，他会随着组件的销毁而销

  ```js
  const app = new Vue({
    template: '<div id="root">{{text}}</div>',
    data: {
      text: 0
    },
    watch: {
      text(newVal, oldVal){
        console.log(`${newVal} : ${oldVal}`);
      }
    }
  })
  ```
  但是，如果我们使用下面这样的方式写 watch，那么就要手动注销了，这种注销其实也很简单
  
  ```js
  const unWatch = app.$watch('text', (newVal, oldVal) => {
    console.log(`${newVal} : ${oldVal}`);
  })
  unWatch()
  ```
  #### 5.注意，不能使用箭头函数 来定义  watcher 函数


