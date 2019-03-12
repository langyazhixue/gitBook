### this.$refs 和 this.$el的作用和区别

#### 先谈谈$refs和 ref
* ref 被用来给元素货自组件注册引用信息，引用信息将会注册在父组件的`$refs`对象上。
如果在普通的DOM元素上使用，引用指向的就是DOM元素；如果在子组件上，引用就指向组件实例

```js
<!-- vm.$refs.p will be the DOM node-->
<p ref='p'>hello</p>
<!-- vm.$refs.child will be the child comp instance-->
<child-comp ref = 'child'><child-comp/>
```

ref 需要在dom渲染完成后才会有，在使用的时候确保`DOM`已经渲染完成。比如生命周期`mounted(){}`钩子中调用，
或者在`this.nextTick(() =>{})`中调用 



* `vm.$refs` 这是一个对象，持有已注册过`ref`的所有子组件



#### $el 
`$el` 是Vue 实例使用的根 DOM 元素,在 `beforeMount` 中才能找到

#### this.$refs 和 this.$el的区别