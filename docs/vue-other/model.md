### 谈谈model以及v-model指令
#### model
* 类型：`{ prop?: string, event?: string}`
* 详细
  允许一个自定义组件在使用 `v-model` 时定制 `prop` 和 `event`。
  默认情况下，一个组件上的 ` v-model` 会把 `value`用作 `prop` 并且把`input` 用作event,
  但是一些输入类型比如说单选框和复选框按钮可能想使用`value` prop来达到不同的目的。
  使用`model`选项可以回避这些情况产生的冲突。

#### 结合v-model指令讲解model的具体用法，主要包括HTML元素的v-model和组件上的v-model两种，用四个简单的案例介绍v-model的使用。

* HTML元素的v-model -输入框(text)

```html
<template>
  <div>
    <input type="text" v-model='price'><!-- 下行注释的语法糖 -->
    <!-- <input :value="price" @input="price = $event.target.value"> -->
  </div>
</template>
<script>
export default {
  name:'baseInput',
  data(){
    return {
      price:20
    }
  }
}
</script>
```

* 定制组件的v-model - 输入框(text)

子组件
```html
<template>
  <div>
    <div><span>{{value}}</span><input type="text" ref="input" :value="value" @input="doThis"/></div><!-- 下行注释的语法糖 -->
  </div>
</template>
<script>
export default {
  name:'componentInput',
  props:{
    value:{
      type:String
    }
  },
  data(){
    return {
    }
  },
  methods:{
    doThis() {
      this.$emit('input', this.$refs.input.value)
    }
  }
}
</script>
```
父组件

```html
<template>
  <div class="test-container">
    <component-input v-model='price'/>
    <!-- <component-input :value="price" @input="val => {price = val}"></component-input> -->
  </div>
</template>
<script>
import componentInput from './componentInput.vue'
export default {
  name: 'TestTwo',
  components:{
    componentInput
  },
  data() {
    return {
      
      price:'20'
    }
  }
}
</script>
```

* 定制组件的v-model - 复选框(checkbox) - 2.2.0 新增 `model上场了`

子组件
```html
<template>
  <div>
    <div>
      <span>{{value}}</span>
      <input type="checkbox" ref="checkbox" :checked="checked" @change="doThis" :value='value'/>
    </div>
  </div>
</template>
<script>
export default {
  name:'componentCheckbox',
  model: {
    prop: 'checked',
    event: 'change'
  },
  props:{
    checked:{
      type:Boolean
    },
    value:{
      type:String
    }
  },
  data(){
    return {
    }
  },
  methods:{
    doThis() {
      this.$emit('change', !this.checked)
    }
  }
}
</script>
```

父组件

```html
<template>
  <div class="test-container">
    {{fruit}}<!-- 观测数据变化 -->
    <component-checkbox v-model="fruit.apple" value="apple"></component-checkbox><!-- 下行注释的语法糖 -->
    <!-- <mcomponent-checkbox :checked="fruit.apple" @change="val => {fruit.apple = val}" value="apple"></mcomponent-checkbox> -->

    <component-checkbox v-model="fruit.peach" value="peach"></component-checkbox><!-- 下行注释的语法糖 -->
    <!-- <mcomponent-checkbox :checked="fruit.peach" @change="val => {fruit.peach = val}" value="peach"></mcomponent-checkbox> -->
  </div>
</template>
<script>
import componentCheckbox from './componentCheckbox.vue'
export default {
  name: 'TestTwo',
  components:{
    componentCheckbox
  },
  data() {
    return {
      fruit: {//数据
        apple: true,
        peach: false
      }
    }
  }
}
</script>
```

* 定制组件的v-model - 单选按钮(radio) -2.2.0 新增

子组件

```html 
<template>
  <div>
    <div>
      <span>{{value}}</span>
      <input type="radio" ref="radio" :name='name' :checked="checked===value" @change="doThis" :value='value'/>
    </div>
  </div>
</template>
<script>
export default {
  name:'componentCheckbox',
  model: {
    prop: 'checked',
    event: 'change'
  },
  props:{
    checked:{
      type:String
    },
    value:{
      type:String
    },
    name:{
      type:String
    }
  },
  data(){
    return {
    }
  },
  methods:{
    doThis() {
      this.$emit('change', this.$refs.radio.value)
    }
  }
}
</script>
```

父组件

```html
<template>
  <div class="test-container">
    {{fruit}}<!-- 观测数据变化 -->
    <component-checkbox v-model="fruit" value="apple" name="myFruit"></component-checkbox><!-- 下行注释的语法糖 -->
    <!-- <mcomponent-checkbox :checked="fruit" @change="val => {fruit = val}" value="apple"></mcomponent-checkbox> -->
    <component-checkbox v-model="fruit" value="peach" name="myFruit"></component-checkbox><!-- 下行注释的语法糖 -->
    <!-- <mcomponent-checkbox :checked="fruit" @change="val => {fruit = val}" value="peach"></mcomponent-checkbox> -->
  </div>
</template>
<script>
import componentCheckbox from './componentCheckbox.vue'
export default {
  name: 'TestTwo',
  components:{
    componentCheckbox
  },
  data() {
    return {
      fruit: 'peach'
    }
  }
}
</script>
```

