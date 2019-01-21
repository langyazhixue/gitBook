
### 开发注意事项
### 1. 组件
* 在Vue中,父子组件的关系可以总结为props down,events up。父组件通过props向下传递数据给子组件，子组件通过events给父组件发送消息
* 每个组件必须只有一个根元素
* 使用 v-bind 来动态传递 prop 
* data 必须是一个函数
* repeat 用 v-for
* 通过事件向父组件传递消息
  * $emit 方法并传入事件的名字，来向父级组件触发一个事件消息
  * 在组件上使用v-model
* props:名字形式为camelCase 的 props 用作特性的时候，需要转换为kebab-case形式(短横线隔开)
* props传递数值,不能使用字面量传递，使用动态props,在data 返回的函数中设置对应的数字
* JS中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它会影响父组件的状态
  * 使用变量储存prop的初始值，并使用watch来观察prop的值的变化。发生变化时，更新变量的值

* 组件名字为camelCase的时候，在模板中使用的时候需要转换为kebab-case形式(短横线隔开)

### 2. 父子组件通信

  * this.$emit 向父组件传递消息 
  * props 父组件向子组件传值 
  * provider/inject 
    * provider/inject: 简单的来说就是在父组件中通过 provider 来提供变量，然后在子组件中通过inject 来注入变量。
    * 需要注意的是这里不论子组件有多深，只要调用了inject那么就可以注入provider的数据。而不是局限于只能从当前父组件的prop属性来获取数据
    
  * this.$parent 子组件访问父组件 
  * this.$root 访问根组件
    * 在初始化web app的时候，给data添加一个 名字为eventhub 的空vue对象
    ```js
    new Vue({
      el: '#app',
      router,
      render: h => h(App),
      data: {
        eventHub: new Vue()
      }
    })
    ```
    这个时候 你就可以一劳永逸了，在任何组件都可以调用事件发射 接受的方法了
    * 某一个组件内调用事件触发
    ```javascript
    //通过this.$root.eventHub获取此对象
    //调用$emit 方法
    submit() {
      //事件名字自定义，用不同的名字区别事件
      //this.$root.Bus.$emit('eventName', 123)
    }
    ```
    * 另一个组件内监听这个事件,当然在组件销毁时接触绑定,使用$off方法
    ```javascript
    // 当前实例创建完成就监听这个事件
      created(){
        this.$root.Bus.$on('eventName', value => {
          this.print(value)
        })
      },

      methods: {
        print(value) {
          console.log(value)
        }
      },

      // 在组件销毁时别忘了解除事件绑定
      beforeDestroy() {
        this.$root.Bus.$off('eventName')
      },
    ```
  * this.$children 父组件的访问子元素
  * this.$refs.child  子组件索引

 


