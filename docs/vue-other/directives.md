###  谈谈Vue的指令

---
vue 自带的指令有很多，比方说`v-text` `v-html` `v-show` `v-if` `v-else` `v-else-if` `v-for` `v-on` `v-bind`等等，前面几个指令很简单，我们重点来看看一下几个指令
&ensp;
<font color="#006600">#</font> **v-on** 缩写 **@**

* vue给v-on内置来很多的修饰符
  * `.stop` - 调用 event.stopProgragation()
  * `.prevent`- 调用 `event.preventDefautl()`
  * `.capture` - 添加事件侦听器时使用`capture`
  * `.self`- 只当事件时从侦听器绑定的元素本身触发时才触发回调
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


&ensp;
<font color="#006600">#</font> **v-model** 
* 预期：随表单控件类型不同而不同
* 限制：
  * `<input>`
  * `<select>`
  * `<textarea>`
  * components

* 修饰符
  * `.lazy` - 取代 `input` 监听 `change`事件
  * `.number` - 输入字符串转为有效的数字
  * `.trim` - 输入首尾空格过滤

关于`v-model` 的具体内容，还可以看[谈谈model以及v-model指令](./model.md) 这篇文章

&ensp;
<font color="#006600">#</font> **v-slot** 

&ensp;
<font color="#006600">#</font> **v-pre** 
* 不需要表达式
* 用法：跳过这个元素和它的子元素的编译过程。可以用来显示原始Mustache标签。跳过大量木有指令的节点会加快编译
* demo

```js
<span v-pre>{{ this will not be compiled }}</span>
```


&ensp;
<font color="#006600">#</font> **v-cloak** 
* 不需要表达式
* :tada: 用法：这个指令保持在元素上直到关联实例结束编译。和 css 规则 如 `[v-cloak]{ display: none}` 一起用时，这个指令可以隐藏未编译的  Mustache 标签直到实例准备完毕。
* demo

```js
<span v-clock>{{ this will not be compiled }}</span>
```

&ensp;
<font color="#006600">#</font> **v-once** 
* 不需要表达式
* :tada: 详细
只渲染元素和组件一次，随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于性能优化

> 再说一次，试着不要过度使用这个模式。当你需要渲染大量静态内容时，极少数的情况下它会给你带来便利，除非你非常留意渲染变慢了，不然它完全是没有必要的——再加上它在后期会带来很多困惑。例如，设想另一个开发者并不熟悉 v-once 或漏看了它在模板中，他们可能会花很多个小时去找出模板为什么无法正确更新。