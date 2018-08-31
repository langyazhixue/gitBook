## plugin
* Plugin 用于扩展 Webpack 的功能，各种各样的Plugin几乎可以让Webpack做任何与构建相关的事情
### webpack自带的常用插件
1. `webpack.optimize.CommonsChunkPlugin`
  * 作用：CommonsChunkPlugin 主要是用来提取第三方库和公共模块，避免首屏的bundle文件或者按需加载的bundle文件体积过大，从而导致加载时间过长，是优化的一把利器

  * 对与这个插件的具体配置和用法可以参照下面：

  参考文章：[详解CommonChunkPlugin配置和用法](https://segmentfault.com/a/1190000012828879)

  有几个问题我一直搞不明白：最近写了几个例子终于搞懂了，不知道大家有没有疑问
* 问题1: 如何理解CommonsChunkPlugin配置设置为minChunks= Infinity?
    ```js
      entry:{
        vendor:["jquery",'other-lib'],
        app:'./entry'
      },
      plugins:[
        new webpack.optimize.CommonChunkPlugin({
          name:["vendor",'runtime'],
          filename: '[name].js',
          minCkunks:Infinity
        })
      ]
    ```
    Infinity 主要用来子第三方库中分离自定义的公共模块，上面的配置中此时的runtime.js 中就是webpack的运行文件
    这里是为了保证 vendor entry 的模块都只放进vendor.js里。
    如果你后面又提取一遍common chunks，叫com1,minChunks 设为2， 恰好有两个模块引用了jquery,但com1并不会把jquery打包进去
    * 问题 2
      minChunks 设置为数字：公共模块被使用的最小次数，例如配置3，即同一模块只有被3个以外的页面同时引用才提出来的common Chunks 
  2. `webpack.DefinePlugin`
    * 作用： 可以在编译时期创建全局变量。该特性适用于开发版本同线上版本在某些常量上有区别场景
    ```js 
    plugins: [
      new webpack.DefinePlugin({
        'process.env': env
      }),
    ]

    ```
  3.  `webpack.HashedModuleIdsPlugin()`
    作用：避免由于引入了一个新模块，使得打包过程中部分模块的ID发生了改变。简单说来，这个插件的用处是为了第三方库在自定义模块更新或者增加的时候，最后打包出来的chunkHash（vender文件）和之前保持一致。这样可以使得 vender可以被缓存在客户端，从而加少客户端的下载量
    参考文章[webpack2中的NamedModulesPlugin与](https://www.jianshu.com/p/cbc5d602f0ee)

  4. `webpack.optimize.ModuleConcatenationPlugin()`
    作用：过去 webpack 打包时的一个取舍是将 bundle 中各个模块单独打包成闭包。这些打包函数使你的 JavaScript 在浏览器中处理的更慢。相比之下，一些工具像 `Closure Compiler` 和 `RollupJS` 可以提升(hoist)或者预编译所有模块到一个闭包中，提升你的代码在浏览器中的执行速度
  5. 

  ### 非webpack自带的插件
  
  1. ` copy-webpack-plugin ` 
    作用: 在webpack中拷贝文件和文件夹
  ```
  | 属性      | 默认值                      | 描述      | 
  | ---------- | ---------------------------- | ---------- |
  | to | to: __dirname + dist | 定义要拷贝的源目录 | 
  | from | from:__dirname + src/public | 定义要拷贝的目录目标 | 
  | toType | file 或者 dir | 可选，默认是文件 | 
  | force | 默认 false | 强制覆盖先前的插件 | 
  | flatten | 默认 false | 只拷贝文件不管文件夹 | 
  | ignore | 可以用模糊匹配 | 忽略拷贝指定的文件 |

  ``` 
    
    
  2. ` html-webpack-plugin `
   作用：生成html文件
  ```js
    new htmlWebpackPlugin({
      title:'用插件生成一个html文件', // 生成html文件的标题
      filename: 'index.html', // 文件名
      template: 'index.html', // 指定你生成的文件所依赖的哪一个html文件模版
      inject: 'head', //script 标签位于html文件的head中
      favicon:'path/to/my_favicon.ico' //给生成的html文件生成一个
      minify:{  //使用minify会对生成的html文件进行压缩
        removeAttributeQuotes: true
      },
      chunksSortMode:'dependency' // script的顺序,按照不同文件的依赖关系来排序
    })
  ```
  3. `extract-text-webpack-plugin`
  作用：抽离css样式，防止将样式打包在js中引起页面样式加载错乱
  ```js
  const ExtractTextPlugin = require("extract-text-webpack-plugin");
    module.exports = {
      module: {
        rules: [
          {
            test: /\.css$/,
            use: ExtractTextPlugin.extract({
              fallback: "style-loader", // 需要什么样的loader去编译文件，
              use: [
                "css-loader",
                'autoprefixer-loader',
                'less-loader'
              ]
            })
          }
        ]
      },
      plugins: [
        new ExtractTextPlugin({
          fileName: '[name].[cotenthash:8].css'  // '生成的文件名, 会包含[name][id][cotenthash][chunkhash]'
          allChunks: true,//  向所有额外的chunk提取
          disable:false,//禁用插件
        }),
      ]
    }
    //注：Loader可以看作是具有文件转换功能的翻译员
  ```
  注：在这里分享别人的一片文章 [详解webpack中的hash、chunkhash、contenthash区别](https://www.jb51.net/article/132275.htm)
  
  4. `optimize-css-assets-webpack-plugin`
  作用：用户弥补 `extract-text-webpack-plugin`的不足
  ```js
  var OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin');
    module.exports = {
      module: {
        rules: [
          {
            test: /\.css$/,
            loader: ExtractTextPlugin.extract('style-loader', 'css-loader')
          }
        ]
      },
      plugins: [
        new ExtractTextPlugin('styles.css'),
        new OptimizeCssAssetsPlugin({
          assetNameRegExp: /\.optimize\.css$/g, // 匹配文件的正则
          cssProcessor: require('cssnano'), // 默认压缩工具
          cssProcessorPluginOptions: {
            preset: ['default', { discardComments: { removeAll: true } }],
          },
          canPrint: true
        })
      ]
    };
  ``` 
  5. `uglifyjs-webpack-plugin`
  作用：压缩JS代码 
   ```js
   const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
    module.exports = {
      plugins: [
        new UglifyJSPlugin({
          compress:{
            warnings:false
          },
          sourceMap:false //使用SourceMaps将错误信息的位置映射到模块，减慢编译的速度
        })
      ]
    };

   ```
  6. `webpack-merge`

  

