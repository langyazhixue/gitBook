#scss 入门
* 最近看Element UI 的源码，然后发现Element UI 的样式是用scss 写的，所以就想着学学 scss


[中文官网](https://www.sass.hk)


[英文官网](./imgs/scss.jpg)

## 安装
* npm install node-sass
* npm install sass-loader

## 编译
* sass --watch scss.scss:test.scss --style expanded

## 启动命令行
* sass -i 

## 语法

### 4中不同的输出方式

输入

```scss
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  li { display: inline-block; }
  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

* 嵌套输出方式 nested

输出

```css
nav ul {
  margin: 0;
  padding: 0;
  list-style: none; }
nav li {
  display: inline-block; }
nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none; }

```


* 展开输出方式 expanded
  
输出

```css
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
nav li {
  display: inline-block;
}
nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}

```

* 紧凑输出方式 compact

输出

```css
nav ul { margin: 0; padding: 0; list-style: none; }
nav li { display: inline-block; }
nav a { display: block; padding: 6px 12px; text-decoration: none; }

```
* 压缩输出方式 compressed

输出

```
nav ul{margin:0;padding:0;list-style:none}nav li{display:inline-block}nav a{display:block;padding:6px 12px;text-decoration:none}

```

### 变量
* 普通变量
输入

```scss
$width:800px;
.header{
  width:$width
}
```

输出

```css
.header {
  width: 800px;
}

```
* 默认变量
  *  sass 的默认变量仅需要在值后面加上 !default 即可
  * sass 的默认变量一般是用来设置默认值，然后根据需求来覆盖的，覆盖的方式也很简单，只需要在默认变量之前重新声明下变量即可。
  * 默认变量的价值在进行`组件化开发`的时候会非常有用
输入

```scss
$baseLineHeight: 2;
$baseLineHeight: 1.5 !default;
body{
    line-height: $baseLineHeight; 
}

```

输出

```css
body {
  line-height: 2;
}

```
* 局部变量和全局变量
  * 在选择器、函数、混合宏...的外面定义的变量为全局变量
  * 而定义在元素内部的变量 就是局部变量
  * 局部变量只会在局部范围内覆盖全局变量

### 嵌套
  `&` 代表 父元素
  * 选择器嵌套
输入

```scss
nav {
  a {
    color: red;

    header & {
      color:green;
    }
  }  
}
```

输出

```css
nav a {
   color: red;
}

header nav a {
   color: green;
}
```

* 属性嵌套
输入

```scss
.box {
  border: {
   top: 1px solid red;
   bottom: 1px solid green;
  }
}
```

输出

```css
.box {
  border-top: 1px solid red;
  border-bottom: 1px solid green;
}
```

* 伪类嵌套
  * 其实伪类嵌套和属性嵌套非常类似，只不过它需要借助`&`符号一起配合使用。

  输入

  ```css
  .clearfix{
  &:before,
  &:after {
      content:"";
      display: table;
    }
  &:after {
      clear:both;
      overflow: hidden;
    }
  }
  ```

  输出

  ```css
  clearfix:before, .clearfix:after {
    content: "";
    display: table;
  }
  .clearfix:after {
    clear: both;
    overflow: hidden;
  }
  ```

### 混合宏
  * 当你的样式变得越来越复杂，需要重复使用大段的样式时，使用变量就无法达到目的了
  * 混合宏可以带传参数
  * 带多个参数可以使用 `...` 来代替

输入

```scss
@mixin border-radius($radius:5px){
  -webkit-border-radius: $radius;
  border-radius: $radius;
}
.header{
  @include border-radius(10px)
}

```

输出

```css
.header {
  -webkit-border-radius: 10px;
  border-radius: 10px;
}
```

### 继承
* 继承类样式块所有样式代码，从而编译出来的css会将选择器合并在一起，形成组合选择器

输入

```scss
.btn {
  border: 1px solid #ccc;
  padding: 6px 10px;
  font-size: 14px;
}

.btn-primary {
  background-color: #f36;
  color: #fff;
  @extend .btn;
}
```

输出

```css
.btn, .btn-primary {
  border: 1px solid #ccc;
  padding: 6px 10px;
  font-size: 14px;
}

.btn-primary {
  background-color: #f36;
  color: #fff;
}

```

### 占位符
* Sass 中的占位符 %placeholder 功能是一个很强大，很实用的一个功能，这也是我非常喜欢的功能。他可以取代以前 CSS 中的基类造成的代码冗余的情形。因为 %placeholder 声明的代码，如果不被 @extend 调用的话，不会产生任何代码。

输入

```scss
%mt5 {
  margin-top: 5px;
}
%pt5{
  padding-top: 5px;
}

.btn {
  @extend %mt5;
  @extend %pt5;
}

```

输出

```css
.btn {
  margin-top: 5px;
}

.btn {
  padding-top: 5px;
}
```
* 混合宏 VS 继承 VS 占位符 
  1. 混合宏
    *  缺点：他不会自动合并相同的样式代码，如果在样式文件中调用同一个混合宏，会产生多个对应的样式代码，造成代码的冗余
    * 优点： 可以传参数
  2. 继承 ：
    * 缺点：不能传参数，不调用的时候有一个基类存在
    * 优点：继承出来的 CSS 会将使用继承的代码块合并到一起，通过组合选择器的方式向大家展现
  3. 占位符
    * 缺点：不能传参数
    * 优点：占位符是独立定义，不调用的时候是不会在 CSS 中产生任何代码；

### 插值 #{}
  * 利用插值可以获得更好的结构体系
输入

``` scss
$properties: (margin, padding);
@mixin set-value($side, $value) {
    @each $prop in $properties {
      #{$prop}-#{$side}: $value;
    }
}
.login-box {
    @include set-value(top, 14px);
}

```

输出

``` css
.login-box {
  margin-top: 14px;
  padding-top: 14px;
}

```

### 数据类型 & 运算
  * 数据类型
    1. 数字: 如，1、 2、 13、 10px；
    2. 字符串: 有引号字符串或无引号字符串，如，"foo"、 'bar'、 'baz'；
    3. 颜色: 如，blue、 #04a3f9、 rgba(255,0,0,0.5)；
    4. 布尔型: 如，true、 false；
    5. 空值: 如，true、 false；
    6. 值列表: 用空格或者逗号分开，如，1.5em 1em 0 2em 、 Helvetica, Arial, sans-serif。

  * 运算

    * 乘法：进行乘法运算时，两个值单位相同时，只需要为一个数值提供单位即可

    ```scss
    box {
      width: 10px * 2;
    }

    ```

    * 除法

    ```scss
    .box {
      width: 100px / 2;
    }

    ```

    *  变量计算
     输入

    ```scss
    .box {
      width: ((220px + 720px) - 11 * 20px ) / 12 ;  
    }
    ```

     输出

    ```css
    .box {
      width: 60px; 
    }

    ```

    * 颜色运算
      
      输入

      ```scss
      p {
        color: #010203 + #040506;
      }
      ```
      输出

      ```css
      p {
        color: #050709;
      }
      ```
    
    * 字符串运算
      * 通过加法符号‘+’来对字符串进行连接

      输入

      ```scss
      $content: "Hello" + "" + "Sass!";
        .box:before {
        content: " #{$content} ";
      }
      ```
      输出

      ```css
      .box:before {
        content:" HelloSass! "
      }
      ```

      * 除了在变量中做字符串连接运算之外，还可以直接通过 + ，把 字符串连接在一起输入

      ```scss
      div {
        cursor: e + -resize;
      }

      ```
      输出

      ```css
      div {
        cursor: e-resize;
      }
      ```
    


  ###  控制命令
  * @if 

  输入
  ```scss
  @mixin blockOrHidden($boolean:true){
    @if $boolean {
      display: block
    }
    @else {
      display: none
    }
  }
  .block {
    @include blockOrHidden
  }

  ```
  输出

  ```css
  .block {
    display: block;
  }
  ```

  * @for
    1. @for 有 **2中** 循环方式
    ```scss
    @for $i from <start> through <end>
    @for $i from <start> to <end>
    ```
    2. 区别
      这两个的区别是关键字 through 表示包括 end 这个数，而 to 则不包括 end 这个数。

    输入
    ```scss
    @for $i from 1 through 3 {
      .item-#{$i} { width: 2em * $i; }
    }

    ```
    输出

    ```css
    .item-1 {
      width: 2em;
    } 
    .item-2 {
      width: 4em;
    }
    .item-3 {
      width: 6em;
    }
    ```   
  * @while

    1. @while指令跟 @for 指令很相似，只要 @while 后面的条件为 true 就会执行

     输入

    ```scss

    $types: 4;
      $type-width:20px;

      @while $types > 0 {
        .while-#{$types} {
          width: $type-width + $types;
      }
      $types: $types - 1
    }

    ```

    输出

    ```css
    .while-4 {
      width: 24px;
    }
    .while-3 {
      width: 23px;
    }
    .while-2 {
      width: 22px;
    }
    .while-1 {
      width: 21px;
    }
    ```

  * @each
    1. @each 循环就是去遍历一个列表，然后从列表中取出对应的值

  输入

  ```scss
  @mixin author-images{
    @each $author in $list {
      .photo-#{$author} {
          background: url("/images/avatars/#{$author}.png") no-repeat;
      }
    }
  }
  .author-bio {
    @include author-images;
  }

  ```
  输出

   ```css
   .author-bio .photo-adam {
    background: url("/images/avatars/adam.png") no-repeat; 
    }
  .author-bio .photo-john {
    background: url("/images/avatars/john.png") no-repeat; 
  }
  .author-bio .photo-wynn {
    background: url("/images/avatars/wynn.png") no-repeat; 
  }
  .author-bio .photo-mason {
    background: url("/images/avatars/mason.png") no-repeat; 
  }
  .author-bio .photo-kuroir {
    background: url("/images/avatars/kuroir.png") no-repeat; 
  }

  ```

  ### 函数
  1. 字符串函数:
    字符串函数顾名思意是用来处理字符串的函数
    * unquote($string)：删除字符串中的引号;
    * quote($string)：给字符串添加引号
    * to-upper-case()：函数将字符串小写字母换成大写字母
    * to-upper-case()：函数将字符串大写字母换成小写字母
  2. 数字函数
    * percentage()：将一个不带单位的数字转换成百分比形式
    * round(）：函数将一个数字四舍五入为一个最接近的整数
    * ceil（）：函数将一个数转换成最接近于自己的整数，会将一个大于自身的任何小数转换成大于本身 1 的整数
    * floor()： 函数刚好与 ceil() 函数功能相反
    * abs() ： 函数会返回一个数的绝对值
    * min() ：多个数之中找到最小
    * max() ：多个数之中找到最大
  3. 列表函数
    * length()：主要用来返回一个列表中有几个值
    * nth($list,$n)：nth() 函数用来指定列表中某个位置的值。不过在 Sass 中，nth() 函数和其他语言不同，1 是指列表中的第一个标签值，2 是指列给中的第二个标签值
    * join():两个列表连接合并成一个列表
    * append():函数是用来将某个值插入到列表中，并且处于最末位。
      ```scss
        $list: append(10px 20px ,30px)
      ```
    * unit():主要用来获取一个值所使用的单位
    * unitless()：判断一个值是否带有单位
    * sass 的 map 常常被称为数据地图,我们可以使用map来管理变量，在sass中，map 自带了七个函数
      ```scss
      $map: (
        $key1: value1,
        $key2: value2,
        $key3: value3
      )
    ``` 
      * map-get($map,$key)：根据给定的 key 值，返回 map 中相关的值。
      * map-merge($map1,$map2)：将两个 map 合并成一个新的 map。
      * map-remove($map,$key)：从 map 中删除一个 key，返回一个新 map。
      * map-keys($map)：返回 map 中所有的 key。
      * map-values($map)：返回 map 中所有的 value。
      * map-has-key($map,$key)：根据给定的 key 值判断 map 是否有对应的 value 值，如果有返回 true，否则返回 false。
      * keywords($args)：返回一个函数的参数，这个参数可以动态的设置 key 和 value

  4. 颜色函数
    颜色函数从大的方面主要分为 RGB,HSL和 Opacity 三大函数
    * RGB函数
      1. rgb($red,$green,$blue)：根据红、绿、蓝三个值创建一个颜色；
      2. rgba($red,$green,$blue,$alpha)：根据红、绿、蓝和透明度值创建一个颜色；
      3. red($color)：从一个颜色中获取其中红色值；
      4. green($color)：从一个颜色中获取其中绿色值；
      5. blue($color)：从一个颜色中获取其中蓝色值；
      6. mix($color-1,$color-2,[$weight])：把两种颜色混合在一起。如果指定的比例是 25%，这意味着第一个颜色所占比例为 25%，第二个颜色所占比例为75%。
    * HSL函数
      1. hsl($hue,$saturation,$lightness)：通过色相（hue）、饱和度(saturation)和亮度（lightness）的值创建一个颜色；
      2. lighten($color,$amount)：让颜色变得更亮；
      3. darken($color,$amount)： 让颜色变得更暗；
      4. saturate($color,$amount):通过改变颜色的饱和度值，让颜色更饱和，从而创建一个新的颜色
      5. desaturate():通过改变颜色的饱和度值，让颜色更少饱和，从而创建一个新的颜色
    * Opacity 函数
      1. opacity($color)：获取一个颜色的透明度值，如果颜色没有指定透明度，那么这个函数得到的值都会是1

      2. rgba($color,$opacity)：修改一个参数的透明值
      3. opacity($color,$amount)/fade-in($color,$amount)： 这两个函数是用来对已有颜色的透明度做一个加法运算，会让颜色更加不透明。其接受两个参数，第一个参数是原始颜色，第二个参数是你需要增加的透明度值，其取值范围主要是在 0~1 之间。当透明度值增加到大于 1 时，会以 1 计算，表示颜色不具有任何透明度。
      
  5. 自定义函数

  输入
  ```scss
  $social-colors: (
    dribble: #ea4c89,
    facebook: #3b5998,
    github: #171515,
    google: #db4437,
    twitter: #55acee
  );
  @function colors($color) {
    @if not map-has-key($social-colors, $color) {
      @warn 'No color found for `#{$color}` in $cocial-colors map.Property omitted'
    }
    @return map-get($social-colors,$color);
  }
  .btn-dribble{
    color:colors(dribble)
  }

  ```
  输出
  ```css
  .btn-dribble {
    color: #ea4c89;
  }

  ```
  ### sass 中的 **@** 规则
  * @import  
    @import 扩展了css 的 @import规则，这让它引入scss和sass 文件

    ```scss
    @import 'index.scss'
    ```
  * @media 
  sass 中的 @media 指令和 CSS 的使用规则一样的简单，但它有另外一个功能，可以嵌套在 CSS 规则中。有点类似 JS 的冒泡功能一样，如果在样式中使用 @media 指令，它将冒泡到外面。

  输入
  ```scss
    .sidebar {
    width: 300px;
      @media screen and (orientation: landscape) {
        width: 500px;
      }
    }
  ```

  输出
  ```css
  .sidebar {
    width: 300px;
  }
  @media screen and (orientation: landscape) {
    .sidebar {
      width: 500px;
    }
  }

  ```
  * @extend 继承

  * @at-root
  从字面上解释就是跳出根元素。当你选择器嵌套多层之后，想让某个选择器跳出，此时就可以使用

  输入
  ```scss
    .a {
    color: red;
    .b {
      color: orange;

      .c {
        color: yellow;

        @at-root .d {
          color: green;
        }
      }
    }  
  }
  ```
  输出
  ```scss
    .a {
    color: red;
  }

  .a .b {
    color: orange;
  }

  .a .b .c {
    color: yellow;
  }

  .d {
    color: green;
  }

  ```
  * @debug @warn @error 调试 用的  









    
  

