### 谈谈 template 解决的痛点
* 前端痛点：在jquery年代，基本上都需要使用JS字符串拼接`HTML`元素然后`append`到页面`DOM` 的情况，
一般的写法都是使用`+`号以字符串的形式拼接，如果是短点还好，如果很长很长的话，拼接就会令人崩溃了
* 解决方法：  `<script type=”text/x-template”>` 
它可以在一定程度上解决这个问题，放在type=”text/x-template”中的内容将不会被浏览器解析，不被执行，不被显示，它只是默默地举根隐身站在那里。

* Vue 模版template的四种写法

来个例子吧

```html
<div id="app">
    <h1>我是直接写在构造器里的模板1</h1>
</div>
<template id="demo3">
    <h1 style="color:red">我是选项模板3</h1>
</template>
 
<script type="text/x-template" id="demo4">
    <h1 style="color:red">我是script标签模板4</h1>
</script>
 
<script>
    var vm=new Vue({
      el:"#app",
      data:{
          message:1
      },
      //第2种模板 写在构造器里
      //template:`<h1 style="color:red">我是选项模板2</h1>`
      //第3种模板 写在<template>标签里
      //template:'#demo3'
      //第4种模板 写在<script type="x-template">标签里
      template:'#demo4'
    })
```

