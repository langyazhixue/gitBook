####   谈谈 extends 和 mixins

* 类型：`Object | Function`
* 详细： 允许声明扩展另一个组件(可以是一个简单的选项对象或构造函数)，而无需使用  `Vue.extend`。这主要是为了便于扩展单文件组件
  这跟 `mixins` 类似

其实接下来我想谈谈当 `extends` 和 `mixins` 结合在一起会发生什么

* 继承钩子函数

```html
<template>
  <div>
    <span>{{name}}</span>
    <a-button @click='change'>改变</a-button>
  </div>
</template>
<script>
const extend = {
  created() {
    console.log('extends created')
  },
  watch:{
    name(newVal,oldVal){
      console.log('extends watch')
    }
  }
}

const mixin1 = {
  created() {
    console.log('mixin1 created')
  },
  watch:{
    name(newVal,oldVal){
      console.log('mixin1 watch')
    }
  }
}

const mixin2 = {
  created() {
    console.log('mixin2 created')
  },
  watch:{
    name(newVal,oldVal){
      console.log('mixin2 watch')
    }
  }
}

export default {
  name: 'testView',
  extends: extend,
  mixins: [mixin1,mixin2],
  data(){
    return {
      name:'前面'
    }
  },
  watch:{
    name(newVal,oldVal){
      console.log('mySelf watch')
    }
  },
  methods:{
    change(){
      this.name = '后面'
    }
  }
}
</script>

```

控制台输出

```html
extends created
mixin1 created
mixin2 created
```
点击按钮后输出

```html
extends watch
mixin1 watch
mixin2 watch
mySelf watch

```
  1. 结论：优先调用 mixins 和 extends 继承的父类，extends 触发的优先级更高，相对于是队列
  2. push(extend, mixin1, minxin2, 本身的钩子函数)
  3. 经过测试，watch的值继承规则一样。


* 继承data

```js
const extend = {
  data () {
    return {
      name: 'extend name'
    }
  }
}
const mixin1 = {
  data () {
    return {
      name: 'mixin1 name'
    }
  }
}
const mixin2 = {
  data () {
    return {
      name: 'mixin2 name'
    }
  }
}
// name = 'name'
export default {
  mixins: [mixin1, mixin2],
  extends: extend,
  name: 'app',
  data () {
    return {
      name: 'name'
    }
  }
}
```

```js
//只写出子类，name = 'mixin2 name'，extends优先级高会被mixins覆盖
export default {
  mixins: [mixin1, mixin2],
  extends: extend,
  name: 'app'
}
```

```js
// 只写出子类，name = 'mixin1 name'，mixins后面继承会覆盖前面的
export default {
  mixins: [mixin2, mixin1],
  extends: extend,
  name: 'app'
}
```
  1. 结论： 子类再次声明，data中的变量都会被重写，以子类的为准
  2. 如果子类不声明，data中的变量将会最后继承的父类为准。
  3. 经过测试，props中属性、methods中的方法和computed的值继承规则一样。

> 关于mixins和extend你可以理解为mvc的c(controller)，这一层。
可见通用的成员变量（包括属性和方法）抽象成为一个父类，提供给子类继承，
这样可以让子类拥有一些通用成员变量，然而子类也可以重写父类的成员变量。
这样的整个编程思想就很面向对象，也就是继承性。






