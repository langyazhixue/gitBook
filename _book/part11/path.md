我们在配置webpack的时候，经常会用到node 的 `path模块`,那我们现在就来了解下常用的path模块的功能吧
#### 1. path.resolve([...paths])
- `...paths <string>` 一个路径或路径片段的序列
- 返回: <string>
path.resolve() 方法会把一个路径或路径片段的序列解析为一个绝对路径。<br/>
如果处理完全部给定的 path 片段后还未生成一个绝对路径，则当前工作目录会被用上。<br/>
生成的路径是规范化后的，且末尾的斜杠会被删除，除非路径被解析为根目录。<br/>
如果没有传入 path 片段，则 path.resolve() 会返回当前工作目录的绝对路径。

```js 
  path.resolve('/foo/bar', './baz');
  // 返回: '/foo/bar/baz'

  path.resolve('/foo/bar', '/tmp/file/');
  // 返回: '/tmp/file'
  path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif');
  // 如果当前工作目录为 /home/myself/node，
  // 则返回 '/home/myself/node/wwwroot/static_files/gif/image.gif'
```

#### 2. path.join([...paths])
- ...paths <string> 一个路径片段的序列
- 返回: <string>
path.join() 方法使用平台特定的分隔符把全部给定的 path 片段连接到一起，并规范化生成的路径

```js
  path.join('/foo', 'bar', 'baz/asdf', 'quux', '..');
  // 返回: '/foo/bar/baz/asdf'

  path.join('foo', {}, 'bar');
  // 抛出 'TypeError: Path must be a string. Received {}'
  path.join('src/common', '../assets/logo.png')
  // 返回 src/assets/logo.png
```
注意：
- __dirname：全局变量，存储的是当前文件所在的文件目录
- __filename：全局变量，存储的是当前文件名