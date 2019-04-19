# less 入门

* Less （Leaner Style Sheets 的缩写），是一门 CSS 预处理语言，它扩展了 css 语言，增加了 变量，Mixin,函数等特性 ，使得 css 更容易维护和扩展

## 安装

* 安装 npm install -g less
* 编译 lessc styles.less styles.css

## 语法

### 变量

输入

```
@nice-blue: pink;
#header {
  color: @nice-blue;
}
```

输出

```
#header {
  color: pink;
}
```

### 嵌套

* & 代表父选择器

输入

```
#header {
  background: red;
  .logo {
    width: 300px;
  }
  &:hover {
    background: green;
  }
}
```

输出

```
#header {
  background: red;
  .logo {
    width: 300px;
  }
  &:hover {
    background: green;
  }
}
```

### 嵌套的冒泡

* @media

输入

```
.screen-color {
  color: red;
  @media (min-width: 768px) {
    color: green;
  }
  @media screen and (min-widht: 1200px) {
    color: #fff;
  }
}
```

输出

```
.screen-color {
  color: red;
}
@media (min-width: 768px) {
  .screen-color {
    color: green;
  }
}
@media screen and (min-widht: 1200px) {
  .screen-color {
    color: #fff;
  }
}
```

### 避免被编译 ~

输入

```
#header {
  width: ~"calc(300px-200px)";
}
```

输出

```
#header {
  width: calc(300px-200px);
}
```

### 运算

* px;cm;%;em 都可以运算

输入

```
@nice-blue: pink;
@light-blue: @nice-blue + #111;
#header {
  color: @light-blue;
}
```

输出

```
#header {
  color: #ffd1dc;
}
```

### 变量替换

输入

```
@my-selector: banner;
.@{my-selector} {
  color: red;
}
```

输出

```
.banner {
  color: red;
}
```

### Extend 扩展

* extend 功能就能有效地解决这个代码重复问题
* extend 默认不能扩展 nested selectors
* 你可以通过增加一个 all 关键字来强制 compiler 来包含所有 nested selectors
* extend 往往用于扩展重用一个静态的 class,mixin 则相当于函数的 pattern，用于不同地方被重复使用

  * 默认模式

    输入

    ```
    .footer {
      padding: 20px;
      h2 {
        color: white;
      }
    }
    .feed {
      &:extend(.footer);
    }
    ```

    输出

    ```
    .footer,
    .feed {
      padding: 20px;
    }
    .footer h2 {
      color: white;
    }
    ```

  * all 模式

    输入

    ```
    .footer {
      padding: 20px;
      h2 {
        color: white;
      }
    }
    .feed {
      &:extend(.footer all);
    }
    ```

    输出

    ```
    .footer,
    .feed {
      padding: 20px;
    }
    .footer h2,
    .feed h2 {
      color: white;
    }
    ```

### 混淆

* 当你只是希望在多个 class 中共享一些静态的 style，那么 extend 是一个非常好的选择，但是 minins 在你需要增加一组动态的 style 时变得非常有用。Mixin 是更高一级的抽象，类似于编程中的函数。

  * 不带参数

  输入

  ```
  .borderd() {
    border: 1px solid red;
  }

  .menu {
    color: green;
    .borderd;
  }
  ```

  输出

  ```
  .menu {
    color: green;
    border: 1px solid red;
  }
  ```

  * 带参数

  输入

  ```
  .borderd(@color:red) {
    border: 1px solid @color;
  }

  .menu {
    color: green;
    .borderd(green);
  }
  ```

  输出

  ```
  .menu {
    color: green;
    border: 1px solid green;
  }
  ```

  * @arguments 包含所有被传递进来的参数

  输入

  ```
  .box-shadow(@x:0,@y:0,@blur:1px,@color:#000) {
    box-shadow: @arguments;
  }
  .shadow {
    .box-shadow;
  }
  ```

  输出

  ```
  .shadow {
    box-shadow: 0 0 1px #000;
  }
  ```

  * 模式匹配，可以通过向其传递参数来更改 mixin 的行为

  输入

  ```
  .mixin(dark,@color) {
    color: darken(@color, 15%);
  }
  .mixin(light,@color) {
    color: lighten(@color, 15%);
  }

  @color-new: dark;
  .color-new {
    .mixin(@color-new,red);
  }
  ```

  输出

  ```
  .color-new {
    color: #b30000;
  }
  ```

### 导入

* @import "styles.less"
* reference: 使用该 less 文件但是不输出它
* inline: 包括在源文件中输出，但是不作处理
* css: 将文件视为 css 文件，无论扩展名为什么
* once: 该文件仅可导入一次 (默认)

```css
  @import (reference, css) "styles.less"
```

### 字符串拼接

输入

```
@iconUrl: "/img";
.c-icon(@bgImg) {
  background-image: url(@bgImg);
}

.bg {
  @someImgUrl: "/icon.png";
  .c-icon("@{iconUrl}@{someImgUrl}");
  // .c-icon("@{iconUrl}/icon.png");
}
```

输出

```
.bg {
  background-image: url("/img/icon.png");
}
```

### 运算符

* 比较运算符 < > <= >= =

* 逻辑运算符 when
  * .mixin (@a) when (@media = mobile) { ... }
  * .mixin (@a) when (@media = desktop) { ... }
  * .mixin (@b) when not (@b > 0) { ... }

输入

```
.max(@a) when(@a > 100px) and (isnumber(@a)) {
  width: @a;
}

.banner-width {
  .max(200px);
}
```

输出

```
.banner-width {
  width: 200px;
}
```

### 循环

输入

```
.cont(@count) when (@count > 0) {
  .cont((@count - 1));
  width: (25px * @count);
}

div {
  .cont(7);
}
```

输出

```
div {
  width: 25px;
  width: 50px;
  width: 75px;
  width: 100px;
  width: 125px;
  width: 150px;
  width: 175px;
}
```
