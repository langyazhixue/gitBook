###  谈谈Vue中的watch
Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：侦听属性--`watch`,watch 的用法大致有三种

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