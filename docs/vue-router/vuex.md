## 在 vue-router 中利用钩子函数调用vuex中的数据
想在 vue-router 中 使用 vuex,就像在普通js文件中使用vuex 一样

### 1. 写一个 store
```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
const store = new Vuex.Store({
  state:{
    userName:'xiaoxiao'
  },
  mutations: {
    SET_NAME:(state,payload) => {
      state.userName = payload.userName
    }
  }
})
export default store
```

### 2. 引入store
permission.js

```js
import router from '@/router/index'
import store from '@/store/index' // 引入 store
router.beforeEach((to, from, next) => {
  store.commit({
    type:'SET_NAME',
    userName:'hangzhou'
  })
  next()
})

```
