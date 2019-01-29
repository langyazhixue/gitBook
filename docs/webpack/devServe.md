
## webpack-dev-server
##  描述
  webpack-dev-server是一个小型的node.js Express服务器,它使用webpack-dev-middleware中间件来为通过webpack打包生成的资源文件提供Web服务。它还有一个通过Socket.IO(代理客户端))连接着webpack-dev-server服务器的小型运行时程序。webpack-dev-server发送关于编译状态的消息到客户端，客户端根据消息作出响应。简而言之，就是一个http服务器
  * devSrver(webpack-dev-server)有一个模块热替换功能会在应用程序中替换、添加或删除模块，而无需重新加载页面。只要开启`hot`配置
  这里有一篇文章写的很好[](https://www.cnblogs.com/wmhuang/p/7137480.html)
  
## 配置

```js
// these devServer options should be customized in /config/index.js
  module.exports = {
    devServer: {
      clientLogLevel: 'warning',
      inline:true
      historyApiFallback: {
        rewrites: [
          { from: /.*/, to: path.posix.join(config.dev.assetsPublicPath, 'index.html') },
        ],
      },
      hot: true,
      contentBase: false, // since we use CopyWebpackPlugin.
      compress: true,
      host: HOST || config.dev.host,
      port: PORT || config.dev.port,
      open: config.dev.autoOpenBrowser,
      overlay: config.dev.errorOverlay
        ? { warnings: false, errors: true }
        : false,
      publicPath: config.dev.assetsPublicPath,
      proxy: config.dev.proxyTable,
      quiet: true, // necessary for FriendlyErrorsPlugin
      watchOptions: {
        poll: config.dev.poll,
      }
    }
  }
  

```
