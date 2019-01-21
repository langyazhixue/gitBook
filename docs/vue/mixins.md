### mixins
 * 混入(mixins)是一种分发Vue组件中可复用功能的非常灵活的方式。混入对象可以包含任意组件选项。当组件使用混入对象时，
所有混入对象的选项将被混入该组件本身的选项

例子

```js

  // 定义一个混入对象
  var myMixin = {
    created: function () {
      this.hello()
    },
    methods: {
      hello: function () {
        console.log('hello from mixin!')
      }
    }
  }
  // 定义一个使用混入对象的组件
  var Component = Vue.extend({
    mixins: [myMixin]
  })

  var component = new Component() // => "hello from mixin!"

```


#### 选项合并特点
1. 当组件和混入对象含有同名选项时候，这些选项将以恰当的方式混合。比如，数组对象在内部会进行浅合并(一层属性深度),在和组件的数据发生冲突时候以组件数据优先

```js

var mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'goodbye',
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})

```

2. 同名钩子函数将混合为一个数组，因此都将被调用，另外，混入对象的钩子将在组件自身钩子之前调用。

```js

  var mixin = {
    created: function () {
      console.log('混入对象的钩子被调用')
    }
  }

  new Vue({
    mixins: [mixin],
    created: function () {
      console.log('组件钩子被调用')
    }
  })
  // => "混入对象的钩子被调用"
  // => "组件钩子被调用"

```

3. 值为对象的选项，例如 methods, components 和 directives，将被混合为同一个对象。两个对象键名冲突时,取组件对象的键值对。

```js

  var mixin = {
    methods: {
      foo: function () {
        console.log('foo')
      },
      conflicting: function () {
        console.log('from mixin')
      }
    }
  }
  var vm = new Vue({
    mixins: [mixin],
    methods: {
      bar: function () {
        console.log('bar')
      },
      conflicting: function () {
        console.log('from self')
      }
    }
  })
  vm.foo() // => "foo"
  vm.bar() // => "bar"
  vm.conflicting() // => "from self"

```

### 全局注册混入对象。一旦使用全局混入对象，将会影响到所有之后创建的Vue实例。

```js
  // 为自定义的选项 'myOption' 注入一个处理器。
  Vue.mixin({
    created: function () {
      var myOption = this.$options.myOption
      if (myOption) {
        console.log(myOption)
      }
    }
  })
  new Vue({
    myOption: 'hello!'
  })
  
```



