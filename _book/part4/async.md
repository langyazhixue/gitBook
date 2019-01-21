# async + await 并发或顺序数据请求
## async 函数概述
async函数是Generator函数的语法糖。其返回值为Promise对象。
该Promise对象必须等到函数内部的所有await后的Promise执行完才会发生状态变化。
## async 函数的并发或顺序执行 
- 并发执行
需要同时发出多个请求,以此来提高请求的效率。如下，同时对多个URL发出请求。
    ```javascript
    async function logInOrder(urls) {
          // 并发读取远程URL
          const textPromises = urls.map(async url => { //此处map()的回调函数也是一个async函数，每次遍历都将返回一个Promise对象
            const response = await fetch(url);
            return response.text();
          });
        
          // 按次序输出
          for (const textPromise of textPromises) {
            console.log(await textPromise);
          }
        }
    ```
- 顺序执行
通过一个简单的循环，每次迭代的时候发出请求，下一个请求必须等到上一个请求返回时，因此效率较低。
    ```javascript
    async function loginInOrder (urls) {
      for (const url of url){
        const response = await fetch(url);
        console.log(await response.text());
      }
    }
    ```

## example
一般的模式是采用async函数并发请求数据，然后将返回结果设置到state中。如下：(ps:为了防止某个await后面的Promise Reject导致整个函数中断，
采用`try/catch`包裹)。

```javascript
this.getData = async () => {
      try {
        const devInfo = await get('/api/res/v2/statistics/dev/info');
        //console.log(devInfo);
        const resApplyTrend = await get('/api/res/v2/statistics/resApplyTrend');
        const catalog = await get('/api/res/v2/statistics/catalog');
        const vmWare = await get(`/api/res/v2/statistics/load/vmware?topNum=10`);
        this.setState({
          data: {
            devInfo,
            resApplyTrend,
            catalog,
            vmWare: vmWare.list
          }
        });
      } catch (err) {
        Modal.error({
          title: '网络错误',
          content: err.message,
        });
        throw err;
      }
    };
  }
```
上述代码中await后面的get()将返回一个Promise。