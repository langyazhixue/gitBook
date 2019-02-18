## css 命名规范 -- BEM 思想

#### 什么是BEM
BEM的意思是块(block) 元素(elemnt) 修饰符(modifier)。是由`Yandex`团队提出的一种前端命名方法论。这种巧妙的命名方法让你的css类对其他开发者来说更加透明而且有意义。BEM命名约定更加严格，而且包含更多的信息，它们用于一个团队开发一个耗时的大项目。
#### 命名的约定模式
  ```js
  .block{}  
  .block__element{}  
  .block--modifier{}

  ```
  * .block 代表了更高级别的抽象或组件
  * .block__element 代表.block的后代，用于形成一个完整的.block的整体。
  * .block--modifier代表.block的不同状态或不同版本。
  
  #### 为什么使用两个连字符和下划线而不是一个？
  * 之所以使用两个连字符和下划线，而不是一个，是为了让你自己的块可以用单个连字符来界定

    ```js

    .site-search()
    .site-search__field{} /* 元素 */  
    .site-search--full{} /* 修饰符 */ 
    ```

  #### BEM 命名规范的作用
  *  BEM 的关键是光凭名字就可以告诉其他开发者某个标记是用来干什么的。通过浏览器HTML代码中的class 属性，就能明白模块之间是如何关联的：有一些仅仅是组件，又一些则是组件的子孙或者元素，还有一些是组件的其他形态或者是修饰符。

  * 例子： 用一个 类比/模型 来思考下一下的元素是怎么关联的
    * 常规的写法

      ```html
        <form class="site-search  full">  
          <input type="text" class="field">  
          <input type="Submit" value ="Search" class="button">  
        </form> 
      ```

    * 这些css类名不太精确，不能告诉我们足够的信息。但是用BEM记号法就如以下

       ```html

        <form class="site-search  site-search--full">  
          <input type="text" class="site-search__field">  
          <input type="Submit" value ="Search" class="site-search__button">  
        </form>

       ```
    我们能清晰的看到有个叫site-search的块，他内部是一个叫site-search__field的元素。并且.site-search 另外一种形态叫 site-search--full


 