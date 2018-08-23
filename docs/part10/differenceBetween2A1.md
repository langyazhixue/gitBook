vue 入门中，本文收集一些Vue 2.0版本与1.0版本的不同之处，欢迎大家补充。

下面是目前收集的信息。

* vue2.0自定义指令中没有this指向指令所在对象，这时候我如果想调用this实例内的data或方法怎么办？
  用`bind 函数中的第三个参数` `node.context`
  ```js
    Vue.directive('my-directive', {
      bind: function (el, binding, vnode) {
        el.innerHTML = 'my-directive'
        console.log(el)
        console.log(binding)
        console.log(vnode.context) // 可以获取绑定的vue实例
      },
      update: function (newValue, oldValue) {
        console.log(newValue)
        console.log(oldValue)
      },
      unbind: function (el) {
        // 清理工作
      },
      inserted: function (el) {

      }
  })
  ```
* vue2.0自定义指令中 没有元素指令

* 过滤器
  *  v2.0 把内置过滤器删除了(caplitalize, uppercase, lowercase)等

  *  v2.0第一个参数必须为自身的值，后面可以加任意多的参数

  * vue2.0里 过滤器只能用类似函数的写法`reverseString( ‘I must tell you:')`，括号内是参数，不同于vue1.0的用空格后加参数的写法

  * v-text里用过滤器失效，原因是在vue2.0里 管道符‘|'只能用在mousetache和v-bind中

* vue 2.0 废弃了双向数据绑定，官方给出的解释是这样的
  ```html
  当父组件的属性变化时，将传导给子组件，但是不会反过来。
  这是为了防止子组件无意修改了父组件的状态——这会让应用的数据流难以理解。

  ```
  个人认为满足以下条件的时候再使用双向绑定的props
  1. 组件内部需要修改props。
  2. 组件需要可以由外部在运行时动态控制，而非单纯初始化。
  3. 组件外部需要读取组件内的状态来进行处理
  
* 


