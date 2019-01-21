vue 入门中，本文收集一些Vue 开发中一些常见问题或者注意事项，欢迎大家补充。


下面是目前收集的问题。

* 问题  
  * 双括号在初始化的闪烁
  * 
* 开发注意事项
  * 组件
  * 父子组件通信
  * 对于computed 的理解
  * vue.proptotype
  * vue.use
  * filters
  * elemnet UI 的坑

#### 问题解决方案
#### 1. 双括号在初始化的闪烁

描述：在页面渲染的时，有时候会看到Mustache 标签，然后看到编译后的数据。
解决方案：
  1. 一种是`<div v-text="name"></div>`,将用v-text指令来显示
  2. 另外一种是用v-clock配合css
  ```js
  <div v-cloak>{{name}}</div>
  <style>
  　　[v-cloak]{
  　　　　display:none;
  　　}
  </style>
  ```


#### 开发注意事项
#### 1. 组件
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



#### 2. 父子组件通信

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

#### 3. 对于computed 的理解

  * data 属性初始化 getter setter
  * computed 计算属性初始化，提供的函数将用作属性 vm.reversedMessage 的 getter
  * 当首次获取 reversedMessage 计算属性的值时，Dep 开始依赖收集
  * 在执行 message getter 方法时，如果 Dep 处于依赖收集状态，则判定 message 为 reversedMessage 的依赖，并建立依赖关系
  * 当 message 发生变化时，根据依赖关系，触发 reverseMessage 的重新计算

  ```js
  var vm = new Vue({
    el: '#example',
    data: {
      message: 'Hello'
    },
    computed: {
      // a computed getter
      reversedMessage: function () {
        // `this` points to the vm instance
        return this.message.split('').reverse().join('')
      }
    }
  })
  ```

#### 3. vue.proptotype

#### 4. vue.use
  Vue.use() 安装一个自定义Vue插件
  ```js
    // 创建一个简单的插件 say.js
    var install = function(Vue) {
      if (install.installed) return // 如果已经注册过了，就跳过
      install.installed = true
      Object.defineProperties(Vue.prototype, {
        $say: {
          value: function() {console.log('I am a plugin')}
        }
      })
    }
    module.exports = install
  ```
  然后我们注册这个插件
  ```js
   import say from './say.js'
   import Vue from 'vue'
   Vue.use(say)
  ```
  这样我们就能调用say 方法了
  我们来看看 Vue.use 方法内部的实现
  ```js
    Vue.use = function (plugin) {
    if (plugin.installed) {
      return;
    }
    // additional parameters
    var args = toArray(arguments, 1);
    args.unshift(this);
    if (typeof plugin.install === 'function') {
      plugin.install.apply(plugin, args);
    } else {
      plugin.apply(null, args);
    }
    plugin.installed = true;
    return this;
  };
  ```

  1. 简述上有一篇文章[关于vue use 详解](https://www.jianshu.com/p/89a05706917a)

#### 5. filters
  - 过滤器可以多个传参数，默认第一个参数是为当前的`message的值value`,`自定义的参数是从第二位开始传的`

#### 6. elemnet UI 的坑
  1. timePicker 给组件加上一个`popper-class` 这样时间组弹框位置就可以设置了
  2. form
    * 表单属性要初始化，这样可以在任何时候进行重置
    * model 数据对象中的属性必须与 rules 中的属性对应

    template
    ```html
    <el-form 
      v-show = 'userInfoVisible' 
      :model="selfInfoForm" 
      :rules="selfInfoFormRules" 
      ref="selfInfoForm" 
      label-width="80px"
    >
      <el-form-item label="姓名"  prop='name'> // prop=name 要与 selfInfoForm.name 对应
        <el-input 
        v-model="selfInfoForm.name"  
        auto-complete="off"
        ></el-input>
      </el-form-item>

      <el-form-item label="性别" prop='sex'>
        <el-radio-group v-model="selfInfoForm.sex">
          <el-radio label="M">男</el-radio>
          <el-radio label="F">女</el-radio>
        </el-radio-group>
      </el-form-item>

      <el-form-item label="手机号"  prop="phone">
        <el-input v-model="selfInfoForm.phone" auto-complete="off"></el-input>
      </el-form-item>

      <el-form-item label="邮箱"  prop="mail">
        <el-input v-model="selfInfoForm.mail" auto-complete="off"></el-input>
      </el-form-item>
    </el-from>
    ```
  script
  ```js
  selfInfoFormRules: {
    name: [
      { required: true, message: '请输入姓名', trigger: 'blur' }
    ],
    sex: [
      { required: true, message: '请选择性别', trigger: 'blur' }
    ],
    phone: [
      { required: true, message: '请输入手机号', trigger: 'blur' },
      {validator: validatePhone, trigger: 'blur'}
    ],
    mail: [
      { required: true, message: '请输入邮箱', trigger: 'blur' },
      {validator: validateMail, trigger: 'blur'}
    ]
  }
  ```
  3. select 组件要初始化
 


