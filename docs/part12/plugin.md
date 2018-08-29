·## plugin
* Plugin 用于扩展 Webpack 的功能，各种各样的Plugin几乎可以让Webpack做任何与构建相关的事情
### webpack自带的常用插件
1. webpack.optimize.CommonsChunkPlugin
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
      minChunks 设置为数字：公共模块被使用的最小次数，例如配置3，即同一模块只有被3哥以外的页面同时引用才提出来宗纬common Chunks 
  2. webpack.DefinePlugin 
    * 作用： 可以在编译时期创建全局变量。该特性适用于开发版本同线上版本在某些常量上有区别场景
    ```js 
    plugins: [
      new webpack.DefinePlugin({
        'process.env': env
      }),
    ]

    ```
  3. webpack.HashedModuleIdsPlugin()
    作用：简单说来，这个插件的用处是为了第三方库在自定义模块更新或者增加的时候，最后打包出来的chunkHash（vender文件）和之前保持一致。这样可以使得 vender可以被缓存在客户端，从而加少客户端的下载量
  