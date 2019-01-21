### 怎么给 vue  写一个插件
  Vue.use() 安装一个自定义Vue插件
  ```js
    // 创建一个简单的插件 say.js
    var install = function(Vue) {
      if (install.installed) return // 如果已经注册过了，就跳过
      install.installed = true
      Object.defineProperties(Vue.prototype, {
        $say: {
          value: function() {console.log('I am a plugin')}
        }
      })
    }
    module.exports = install
  ```
  然后我们注册这个插件
  ```js
   import say from './say.js'
   import Vue from 'vue'
   Vue.use(say)
  ```
  这样我们就能调用say 方法了