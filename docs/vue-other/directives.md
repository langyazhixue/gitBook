###  谈谈Vue的指令

---
vue 自带的指令有很多，比方说`v-text` `v-html` `v-show` `v-if` `v-else` `v-else-if` `v-for` `v-on` `v-bind`等等，前面几个指令很简单，我们重点来看看一下几个指令
&ensp;
<font color="#006600">#</font> **v-on()** 缩写 **@**

* vue给v-on内置来很多的修饰符
  * `.stop` - 调用 event.stopProgragation()
  * `.prevent`- 调用 `event.preventDefautl()`
  * `.capture` - 添加事件侦听器时使用`capture`
  * `.self`- 只当事件时从侦听器绑定的元素本身触发时才触发收回
  * `.{keyCode | keyAlias }` - 只当事件是从特定键触发才触发回调
  * `.native`- 监听根元素的原生事件
  * `.once` - 只触发一次回调
  * `.left`- 只当点击鼠标左键的时候触发
  * `.right` - 只当点击鼠标右键时触发
  * `.middle` - 只当点击鼠标中键时触发
  * `.passive` - 以 `{ passive: true}` 模式添加侦听器

* 用法
绑定事件监听器，事件类型由参数指定。表达式可以是一个方法的名字或一个内联语句，如果没有修饰符也可以省略

用在普通元素上时候，只能监听**原生DOM事件**。用在自定义元素组件上时候，也可以监听子组件触发的**自定义事件**

从 2.4.0 开始，v-on 同样支持不带参数绑定一个事件/监听器键值对的对象。注意当使用对象语法时，是不支持任何修饰器的。

* demo
```html
<!-- 方法处理器 -->
<button v-on:click="doThis"></button>

<!-- 动态事件 (2.6.0+) -->
<button v-on:[event]="doThis"></button>

<!-- 内联语句 -->
<button v-on:click="doThat('hello', $event)"></button>

<!-- 缩写 -->
<button @click="doThis"></button>

<!-- 动态事件缩写 (2.6.0+) -->
<button @[event]="doThis"></button>

<!-- 停止冒泡 -->
<button @click.stop="doThis"></button>

<!-- 阻止默认行为 -->
<button @click.prevent="doThis"></button>

<!-- 阻止默认行为，没有表达式 -->
<form @submit.prevent></form>

<!--  串联修饰符 -->
<button @click.stop.prevent="doThis"></button>

<!-- 键修饰符，键别名 -->
<input @keyup.enter="onEnter">

<!-- 键修饰符，键代码 -->
<input @keyup.13="onEnter">

<!-- 点击回调只会触发一次 -->
<button v-on:click.once="doThis"></button>

<!-- 对象语法 (2.4.0+) -->
<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
```
在子组件上监听自定义事件(当子组件触发'my-event'时将调用事件处理器)


```html
<my-component @my-event="handleThis"></my-component>
<!-- 内联语句 -->
<my-component @my-event="handleThis(123, $event)"></my-component>
<!-- 组件中的原生事件 -->
<my-component @click.native="onClick"></my-component>
```




