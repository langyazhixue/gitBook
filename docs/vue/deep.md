### vue scoped scss 与深度作用选择器 /deep/
* 使用 scoped 后，父组件样式将不会渗透到子组件中
* 例如 (无效)


 ```html
 <template>
  <div id="app">
    <el-input  class="text-box" v-model="text"></el-input>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      text: 'hello'
    };
  }
};
</script>
<style lang="less" scoped>
.text-box {
   input {
    width: 166px;
    text-align: center;
  }
}
</style>
 ```

* 解决方案： 使用深度作用选择器 `/deep`

```html
<template>
  <div id="app">
    <el-input v-model="text" class="text-box"></el-input>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      text: 'hello'
    };
  }
};
</script>

<style lang="less" scoped>
.text-box {
  /deep/ input {
    width: 166px;
    text-align: center;
  }
}
</style>
```


