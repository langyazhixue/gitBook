### 谈谈 $parent, $children



#### $parent
* 类型：`Vue instance`
* 指定已创建的实例之父实例，在两者之间建立父子关系，子实例可以用`this.$parent`访问父实例，子实例被推入父实例的`$children`数组中

 注意
 > 节制地使用 $parent 和 $children - 它们的主要目的是作为访问组件的应急方法。更推荐用 props 和 events 实现父子组件通信