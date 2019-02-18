### element UI、iview 等组件库修改样式不污染全局环境

第三方`UI组件库`帮助我们在项目中可以大大提高开发效率，但是有时候我们需要根据UI设计稿修改`UI组件库`的样式。
在`Vue`中，修改第三方组件库，在样式中需要去掉 `scoped`,否则会使得修改样式不生效。但是如果去掉`scoped`，就代表该样式是全局的，
修改的插件样式会影响全局，目前有以下2种方案

#### 1. 我们在组件库样式的外面定义一个父盒子包裹以下

```css
 <style rel="stylesheet/scss" lang="scss" >
  .loginform {
    height: 300px;
    box-sizing: border-box;
    padding:10px 60px 0;
    text-align: left;
    font-weight: 700;
    .el-form-item {
      margin-bottom: 15px;
    }
    .el-form--label-top .el-form-item__label {
      padding: 0;
    }
  }
 </style> 
```

####  2. 使用 /deep/

 ```css
 <style rel="stylesheet/scss" lang="scss" >
  .loginform {
    height: 300px;
    box-sizing: border-box;
    padding:10px 60px 0;
    text-align: left;
    font-weight: 700;
    /deep/ .el-form-item {
      margin-bottom: 15px;
    }
    /deep/ .el-form--label-top .el-form-item__label {
      padding: 0;
    }
  }
 </style>

 ```
这种方式，不需要去掉scoped,所以推荐使用

