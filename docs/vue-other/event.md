###  谈谈Vue的实例方法/属性


---

<font color="#006600">#</font> **vm.$on(event,callback)**  
* 用法：监听当前实例上的自定义事件。事件可以由`vm.$emit`触发。回调函数会接收所有传入事件触发函数的额外参数。

* 
```js
vm.$on('test', function (msg) {
  console.log(msg)
})
vm.$emit('test', 'hi')
```
&ensp;

<font color="#006600">#</font> **vm.$once(event,callback)**  

* :sunflower: 用法：监听一个自定义事件，但是只触发一次，在第一次触发之后移除监听器
&ensp;

<font color="#006600">#</font> **vm.$off(event,callback)** 

* 用法：
  移除自定义事件监听器
  * 如果没有提供参数，则移除所有的事件监听器；
  * 如果只提供了事件，则移除该事件所有的监听器；
  * 如果同时提供了事件与回调，则只移除这个回调的监听器 
&ensp;

<font color="#006600">#</font> **vm.$emit(enentName,[...args])** 

* demo

```js
Vue.component('welcome-button', {
  template: `
    <button v-on:click="$emit('welcome')">
      Click me to be welcomed
    </button>
  `
})
```
```html
<div id="emit-example-simple">
  <welcome-button v-on:welcome="sayHi"></welcome-button>
</div>
```
```js
new Vue({
  el: '#emit-example-simple',
  methods: {
    sayHi: function () {
      alert('Hi!')
    }
  }
})
```
