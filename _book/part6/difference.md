### scss与sass的区别
  Scss 是 Sass 3 引入新的语法，其语法完全兼容 CSS3，并且继承了 Sass 的强大功能。简单来说scss 是sass 的一个升级版本。
1. 语法形式上有些许不同，最主要的就是sass是靠缩进表示嵌套关系，scss是花括号
  ```js
      //sass 太费眼了
    .father
        width:100px;
        .son
            width:50px;
    //scss 适合我这种眼瘸手残患者
    .father{
        width:100px;
        .son{
            width:50px;
        }
    } 
  ```
  2. 文件扩展名不同