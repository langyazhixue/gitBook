### 谈谈Vue中的过渡和动画
Vue 在插入、更新、或者移除DOM时候，提供多种不同方式的应用过渡效果。
包括以下工具：

* 在 CSS 过渡和动画中自动应用`class`
* 可以配合使用第三方css动画库，如`Animate.css`
* 在过渡钩子函数中使用Javascript直接操作DOM
* 可以配合使用第三方Javascript动画库，如 `Velocity.js`

#### 单元素/组件的过渡
---

Vue 提供了`transition`的封装组件，在下列情形下，可以给任何元素和组件添加进入/离开过渡

* 条件渲染(使用 `v-if`)
* 条件展示(使用 `v-show`)
* 动态组件
* 组件根节点

:palm_tree:demo

```html
<h2>简单的过渡2</h2>
  <a-button @click="show1 = !show1">
    Toggle
  </a-button>
  <transition name="slideTest">
    <p v-if="show1">
      hello
    </p>
  </transition>
```

```js
new Vue({
  el: '#demo',
  data: {
    show1: true
  }
})
```

```css
  .slideTest-enter-active {
    transition: all 0.3s ease
  }
  .slideTest-leave-active {
    transition: all 0.8s cubic-bezier(1.0, 0.5, 0.8, 1.0)
  }
  .slideTest-enter {
    transform: translateX(10px);
    opacity: 0
  }
  .slideTest-leave-to {
    transform: translateX(10px);
    opacity: 0
  }
```
当插入/删除包含在 `transition` 组件中的元素时候，Vue会做如下处理：

1. 自动嗅探目标元素是否应用了`css`过渡或动画，如果是，在恰当的时间添加/删除 `css`类名
2. 如果过渡组件提供了`JavaScript钩子函数`，这些钩子函数将在恰当的时候被调用
3. 如果没有找到`JavaScript`钩子并且也没有检测到css过渡/动画，DOM操作(插入/删除)在下一帧立即执行。 (注意：此浏览器逐帧动画机制)

<font color="#006600">#</font>过渡的类名
1. `v-enter`:定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除
2. `v-enter-active`:定义进入过渡生效时的状态。在整个进入过渡的阶段应用，在过渡被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义`进入过渡的过程时间`，`延迟`和`曲线函数`。
3. `v-enter-to`: `2.1.8版及以上` 定义进入过渡的结束状态。在元素被插入之后下一帧生效(与此同时`v-enter`被移除)，在过渡/动画完成之后移除。
4. `v-leave`：定义离开过渡的状态。在离开过渡被触发时立即生效，下一帧被移除
5. `v-leave-active`:定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立即生效，在过渡/动画完成之后移除。这个类可以被用来定义`离开过渡的过程时间`、`延迟`和`曲线函数`
6.  `v-leave-to`: 定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效(与此同时 `v-leave` 被删除),在过渡/动画完成之后移除

![img](./img/transition/transition.png)


如果你使用 `<<transition name="my-transition>`,
那么 `v-enter`会替换成 `my-transition-enter`
`v-enter-active` 和 `v-leave-active` 可以控制进入/离开过渡的不同的缓和曲线

:palm_tree:css动画

```html
<h2>css 动画用法同css过渡，区别是在动画中`v-enter`类名在节点插入DOM后不会立即删除，而是`animationend`事件触发时删除</h2>
<a-button @click="show2=!show2">
  Toggle
</a-button>
<transition name="bounce">
  <p v-if="show2">
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris facilisis enim libero, at lacinia diam fermentum id. Pellentesque habitant morbi tristique senectus et netus.
  </p>
</transition>

 <h2>Vue 过渡系统结合其他第三方css动画库，如 Animate.css 结合</h2>
  <a-button @click="show3=!show3">
    Toggle
  </a-button>
  <transition
    name="custom-classes-transition"
    enter-active-class="animated tada"
    leave-active-class="animated bounceOutRight"
    :duration="{ enter: 500, leave: 800 }"
  >
    <p v-if="show3">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris facilisis enim libero, at lacinia diam fermentum id. Pellentesque habitant morbi tristique senectus et netus.
    </p>
  </transition>
```

```js
new Vue({
  el: '#demo',
  data: {
    show2:true,
    show3: true,
  }
})
```
```css
 .bounce-enter-active {
    animation:  bounce-in 0.5s
  }
  .bounce-leave-active {
    animation:  bounce-in 0.5s reverse
  }
  @keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
```
* 自定义过渡的类名，他们的优先级高于普通的类名，这对于Vue的过渡系统和其他第三方`css`动画库，如
`Animate.css`结合使用十分有用
* 显性的过渡持续时间
```html
  <transition :duration="{ enter: 500, leave: 800 }">...</transition>
```

:palm_tree:Javascript 钩子

```js
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"

  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
</transition>
```
> 当只用 JavaScript 过渡的时候，在 enter 和 leave 中必须使用 done 进行回调。否则，它们将被同步调用，过渡会立即完成。
> 推荐对于仅使用 JavaScript 过渡的元素添加 v-bind:css="false"，Vue 会跳过 CSS 的检测。这也可以避免过渡过程中 CSS 的影响。

:palm_tree:初使渲染的过渡
```html
  <a-button @click="show5=!show5">
    Toggle
  </a-button>
  <transition
    appear
    appear-class='appear-test-class'
    appear-to-class='appear-to-test-class'
    appear-active-class="appear-active-test-class"
  >
    <p v-if="show5">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris facilisis enim libero, at lacinia diam fermentum id. Pellentesque habitant morbi tristique senectus et netus.
    </p>
  </transition>
```

```js
new Vue({
  el: '#demo',
  data: {
    show5:true
  }
})
```

```css
.appear-test-class {
  opacity: 0;
  transform: translateX(10px)
}
.appear-active-test-class {
  transition: all 10s ease
}
.appear-active-test-class {
  opacity: 1
}
```


:palm_tree:多元素的过渡
* transition 默认 -  进入和离开会同时发生
* `in-out`:新元素先进行过渡，完成之后当前元素过渡离开
* `out-in`: 当前元素先进行过渡,完成之后新元素过渡进入
> 当有相同标签名的元素切换时候，需要通过`key`特性设置唯一的值来标记以
让Vue来区分它们，

```html
  <h2>in-out</h2>
  <div class="test--container">
    <transition
      name="modeTest"
      mode="in-out"
    >
      <a-button :key="show6" class="modeTest11" @click="show6=!show6">
        {{ show6 ? 'on' : 'out' }}
      </a-button>
    </transition>
  </div>
```

```js
new Vue({
  el: '#demo',
  data: {
    show6:true
  }
})
```

```css
.test--container {
  position: relative;
  width: 500px;
  height: 500px;
  min-height: 500px;
  border:1px solid red;
  .modeTest11{
    position:absolute;
    top:200px;
    left:200px;
  }
}
.modeTest-enter {
  opacity: 0;
  transform: translateX(200px);
}
.modeTest-enter-active{
  opacity: 1;
  transition:all 0.5s ease;
}
.modeTest-leave-active{
  opacity: 1;
  transition: all 1s ease
}
.modeTest-leave-to{
  transform: translateX(-200px);
  opacity: 0
}
```

:palm_tree:多组件的过渡

多个组件的过渡简单很多-我们不需要使用`key`特性。相反，我们只需要使用`动态组件`

```html
<h2>多个组件过渡</h2>
<a-button @click="toggle">
  toggle
</a-button>
<transition name="component-fade" mode="out-in">
  <component :is="view" />
</transition>
```


```js

new Vue({
  el: '#demo',
  components: {
    'v-a': {
      template: '<div>Component A</div>'
    },
    'v-b': {
      template: '<div>Component B</div>'
    }
  },
  data: {
    view:'v-a'
  },
  methods:{
    toggle() {
      if (this.view === 'v-a') {
        this.view = 'v-b'
      } else {
        this.view = 'v-a'
      }
    }
  }
})
```

```css
.component-fade-enter-active, .component-fade-leave-active {
  transition: opacity .3s ease;
}
.component-fade-enter, .component-fade-leave-to
/* .component-fade-leave-active for below version 2.1.8 */ {
  opacity: 0;
}
```


:palm_tree:列表的过渡
 使用 `<transiyion-group>` 组件，这个组件有以下特点：
 * 不同于 `transiyion`, 它会以一个真实元素呈现，默认为一个`span`。
 也可以通过`tag`特性更换为其他元素

* 过渡模式不可用，因为我们不再相互切换特有的元素

* 内部元素 `总是需要` 提供唯一的 `key`值

```html
<h2>列表的过渡</h2>
<div id="list-demo" class="demo11">
  <a-button v-on:click="shuffle">
    Shuffle
  </a-button>
  <a-button @click="handleradd">
    Add
  </a-button>
  <a-button @click="remove">
    Remove
  </a-button>
  <transition-group name="listdddd" tag="p">
    <span v-for="item in items" :key="item" class="list-item">
      {{ item }}
    </span>
  </transition-group>
</div>
```
```css
.listdddd-enter-active, .listdddd-leave-active {
  transition: all 1s;
  transform: translateY(0px);
  opacity:1
}
.listdddd-enter, .listdddd-leave-to
/* .list-leave-active for below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
```

```js
new Vue({
  el: '#demo',
  components: {
    'v-a': {
      template: '<div>Component A</div>'
    },
    'v-b': {
      template: '<div>Component B</div>'
    }
  },
  data: {
    items:[1,2,3,4,5,6,7,8,9],
    nextNum:10
    view:'v-a'
  },
  methods:{
    randomIndex() {
      return Math.floor(Math.random() * this.items.length)
    },
    handleradd() {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove() {
      this.items.splice(this.randomIndex(), 1)
    },
    shuffle:function(){
      this.items = _.shuffle(this.items)
    },
  }
})
```

:palm_tree:列表的交错过渡
通过`Data`属性和`Javascript`通信，就可以实现列表的交错过渡

```html
<h2>列表的交错过渡</h2>
<div>通过data属性与JS通信，就可以实现列表的交错过渡</div>
<div id='stagered-list-demo'>
  <a-input v-model='query'></a-input>
  <transition-group
    name='stagered-fade'
    tag='ul'
    v-bind:css='false'
    @before-enter='stageredBeforeEnter'
    @enter='stageredEnter'
    @leave='stageredLeave'
    mode='out-in'
  >
    <li
    v-for="(item,index) in computedList"
    :key='item.msg'
    :data-index='index'
    >
    {{item.msg}}
  </li> 
  </transition-group>
</div>
``` 
```js
new Vue({
  el: '#demo',
  data: {
    query: '',
    list: [
      { msg: 'Bruce Lee' },
      { msg: 'Jackie Chan' },
      { msg: 'Chuck Norris' },
      { msg: 'Jet Li' },
      { msg: 'Kung Fury' }
    ]
  },
   computed: {
    computedList: function () {
      var vm = this
      return this.list.filter(function (item) {
        return item.msg.toLowerCase().indexOf(vm.query.toLowerCase()) !== -1
      })
    }
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
      el.style.height = 0
    },
    enter: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 1, height: '1.6em' },
          { complete: done }
        )
      }, delay)
    },
    leave: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 0, height: 0 },
          { complete: done }
        )
      }, delay)
    }
  }
})
```


:palm_tree:动态过渡
用`Vue`的过渡系统来定义的`css`过渡/动画在不同过渡间切换会非常有用







