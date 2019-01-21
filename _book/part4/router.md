# React + React-Router 路由配置

## React-Router
- 概述：Router实质为一个容器，真正的路由通过Route组件来定义。
```html
<Router history={hashHistory}>
    <Route path="/" component={App}/>
  </Router>
```
上面代码的含义是访问根路由`/`时，将加载App组件。组件和路由一一对应。可以添加多个Route组件来定义多组路由。
也可以嵌套路由，实现在父组件中加载子组件。如下，访问`/repos`时，会先加载App，然后在其内部加载Repos.
```html
<Router history={hashHistory}>
    <Route path="/" component={App}/>
        <Route path="/repos" component={Repos}/>
    </Route>
</Router>
```
ps:更多组件参见[这里](https://reacttraining.com/react-router/web/api/)

## 配置方法
在实际项目中，通常将route和component的映射关系单独写在一个文件里。路由可以是同步路由，也可以是异步路由。

- 异步路由的实现

    通过async函数将路由异步化。在匹配到指定路由时才加载对应的组件.如下代码片段：
    
    ```javascript
    [
      {
          path: "/",
          name: "门户",
          getComponent: PoolReport(),
          childRoutes: [{
            path: "home-pool",
            name: "资源池",
            getComponent: HomePool()
          }]
       }, 
      'other route...',
     ]
    ```
    `PoolReport()`和`HomePool()`是异步加载组件的函数，位于要加载组件目录之中。该函数的实现如下:
    先异步import组件，

	```javascript
	async function doGet(){
        try{
         const component = await import('./');
         return component;
        }catch(error){
         console.log(error);
       }
    }
	```
	然后创建组件
	```javascript
	create(errorLoading, injectReducer, injectSagas) {
    	return function(nextState, cb) {
    		doGet()
    			.then(component => {
    				cb(null, component.default);
    			})
    			.catch(errorLoading)
    	}
    }
    ```
    
    这里涉及到将组件的reducer和saga注入到全局。
    
    1.注入reducer的一般做法是采用store对象的`replaceReducer(nextReducer)`来替换当前的reducer。
    `nextReducer`为注入了当前reducer的新reducer。如下：
    
    ```javascript
    export function injectAsyncReducer(store, isValid) {
        return function injectReducer(name, aysncReducer) {
            if(isValid) {
                //...
            }
            invariant(
                  isString(name) && !isEmpty(name) && isFunction(asyncReducer),
                  '(app/utils...) injectAsyncReducer: Expected `asyncReducer` to be a reducer function'
            );
            if (Reflect.has(store.asyncReducers, name)) return;
            store.asyncReducers[name] = asyncReducer;
            store.replaceReducer(createReducer(store.aysnReducer));
        }
    }
    ```
        
    2.注入saga的方式和注入reducer的方式大致相同。采用`sagas.map(store.runSaga)`。

    ```javascript
    export function injectAsyncSagas(store, isValid) {
        return function injectSagas(sagas) {
            if(isValid){
                //...
            }
            invariant(
                  isString(name) && !isEmpty(name) && isFunction(asyncReducer),
                  '(app/utils...) injectAsyncReducer: Expected `asyncReducer` to be a reducer function'
            );
            warning(
                  !isEmpty(sagas),
                  '(app/utils...) injectAsyncSagas: Received an empty `sagas` array'
            );
            sagas.map(store.runSaga); 
        }
    }
    ```
    
- 所有的路由信息可以按照路由嵌套规则以上述的方法配置。然后在应用的入口文件（app.js）中指定根路由和根组件。如下：

    ```javascript
    const rootRoute = {
      component: App,
      childRoutes: createRoutes(store)
    };
    ```
- 然后将根路由添加到路由器Router的route属性中即可。
    ```html
    <Provider store={store}>
      <LanguageProvider messages={translationMessages}>
        <Router history={history} routes={rootRoute}/>
      </LanguageProvider>
    </Provider>
    ```
    