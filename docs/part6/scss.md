#scss 入门
* 最近看Element UI 的源码，然后发现Element UI 的样式是用scss 写的，所以就想着学学 scss
[中文官网](https://www.sass.hk)
![scss](./imgs/scss.jpg)

## 安装
* npm install sass
* npm install sass-loader

## 编译
* sass --watch scss.scss:test.scss --style expanded

## 语法

### 4中不通的输出方式

输入

```css
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

1. 嵌套输出方式 nested

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


2. 展开输出方式 expanded

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

3. 紧凑输出方式 compact

输出

```css
nav ul { margin: 0; padding: 0; list-style: none; }
nav li { display: inline-block; }
nav a { display: block; padding: 6px 12px; text-decoration: none; }

```
4. 压缩输出方式 compressed

输出

```
nav ul{margin:0;padding:0;list-style:none}nav li{display:inline-block}nav a{display:block;padding:6px 12px;text-decoration:none}

```

### 变量
1. 普通变量
输入

```css
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
2. 默认变量
  *  sass 的默认变量仅需要在值后面加上 !default 即可
  * sass 的默认变量一般是用来设置默认值，然后根据需求来覆盖的，覆盖的方式也很简单，只需要在默认变量之前重新声明下变量即可。
  * 默认变量的价值在进行`组件化开发`的时候会非常有用
输入

```css
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
3. 局部变量和全局变量
  * 在选择器、函数、混合宏...的外面定义的变量为全局变量
  * 而定义在元素内部的变量 就是局部变量
  * 局部变量只会在局部范围内覆盖全局变量

### 嵌套
  * `&` 代表 父元素
1. 选择器嵌套
输入

```css
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

2. 属性嵌套
输入

```css
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

3. 伪类嵌套
  * 其实伪类嵌套和属性嵌套非常类似，只不过他需要借助`&`符号一起配合使用。
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

```css
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

```css
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

```css
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
* * 混合宏 VS 继承 VS 占位符 
  1. 混合宏
    *  缺点：他不会自动合并相同的样式代码，如果在样式文件中调用同一个混合宏，会产生多个对应的样式代码，造成代码的冗余，
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

``` css
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
    1. 乘法：进行乘法运算时，两个值单位相同时，只需要为一个数值提供单位即可

    ```css
    box {
      width: 10px * 2;
    }

    ```
    2. 除法

    ```css
    .box {
      width: 100px / 2;
    }

    ```
    3. 变量计算
     输入
    ```
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
    4. 颜色运算
    
    输入

    ```css
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
    5. 字符串运算
    
  

