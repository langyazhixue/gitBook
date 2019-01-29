## webpack3中 hash、chunkhash 和 contenthash 三者的区别
hash 的作用:hash一般是结合CDN缓存来使用，通过webpack构建之后，生成对应文件名自动带上对应的MD5值。如果文件内容改变的话，那么对应文件哈希值也会改变，对应的HTML引用的URL地址也会改变，触发CDN服务器从源服务器上拉取对应数据，进而更新本地缓存。

### 1. hash 
* hash是跟整个项目的构建相关，只要项目里有文件更改，整个项目构建的hash值都会更改，并且全部文件都共用相同的hash值。所以一旦修改了任何一个文件，整个项目的文件缓存都将失效

### 2. chunkhash 
* 如上看到采用 hash的话，每一次构建生产的hash值都不一样，这样不能利用浏览器的缓存功能。我们需要另外一种哈希值计算方式即chunkhash
* chunkhash和hash不一样，它根据不同的入口文件（Entry)进行依赖文件解析，构建对应的chunk。我们在生产环境里把一些公共库和程序入口文件区分开，单独打包构建，接着我们采用chunkhash的方式生成哈希值，那么只要我们不改动公共库的代码，就可以保证其哈希值不会受影响。

* chunkhash的改变
  1. 自身内容的改变
  2. module.id 发生改变

webpack.prod.conf.js 把生产环境中公共库和程序入口文件分开
```js
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendor',
      minChunks (module) {
        // any required modules inside node_modules are extracted to vendor
        return (
          module.resource &&
          /\.js$/.test(module.resource) &&
          module.resource.indexOf(
            path.join(__dirname, '../node_modules')
          ) === 0
        )
      }
    }),
    new webpack.optimize.CommonsChunkPlugin({
      name: 'manifest',
      minChunks: Infinity
    }),
    new webpack.optimize.CommonsChunkPlugin({
      name: 'app',
      async: 'vendor-async',
      children: true,
      minChunks: 3
    })
```
* 在配置的时候用在 js的名称输出上

```js
  output: {
    path: config.build.assetsRoot,
    filename: utils.assetsPath('js/[name].[chunkhash].js')
  }
```

### 1. contenthash
  如果我们在index.js 中引用了index.css,那么index.css和index.js 会共用相同的chunkhash值。但是这样子有个问题，如果index.js更改了代码，css文件就算内容没有任何改变，但是该模块发生了改变，导致css文件会重复构建。这个时候我们需要在`extra-text-webpack-plugin`中的用到contenthash

  webpack.prod.conf.js
  ```js
  const ExtractTextPlugin = require("extract-text-webpack-plugin");

  modules.exports={
    module:{
      rules:[{
        test:/\.csss$/,
        use:ExtractTextPlugin.extract({
          fallback: "style-loader",
          use: ['css-loader','postcss-loader']
        })
      }]
    },
    plugins:[
      new ExtractTextPlugin({
        filename: utils.assetsPath('css/[name].[contenthash].css')
        allChunks: true,
      })
    ]
  }
  ```
  