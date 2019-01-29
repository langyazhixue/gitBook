## Loader 的作用
Loader 可以看做是文件转换功能的翻译员,配置里的module.rules数组配置了一组规则，告诉Webpack 在遇到哪些文件时候使用哪些Loader去加载和转换。
```js
  module: {
    rules:[
      {
        test:'./css$',
        use:[
          'style-loader',
          {
            loader:'css-loader',
            options:{
              minize: true
            }
          }
        ]
      }
    ]
  }
```
如上配置告诉Webpack，在遇到.css结尾的文件时候，先使用css-loader读取css文件，再由style-loader将css的内容注入到js 中。<br/>
注意
 - use 属性的值需要是一个由 Loader 名称组成的数组，Loader 的执行顺序是由后往前的
 - 每个 Loader 都可以通过options 的方式传入参数，也可以通过 URL queryString 的方式传入参数

### Loader简介
#### 1. eslint-loader

 


