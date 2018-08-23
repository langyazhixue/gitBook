# React 入门

本文用了react-boilerplate 脚手架，结合react,redux,redux-saga,immutable等技术栈，结合ant-design 等框架，搭建了一个react 全家桶网站

## 项目目录结构

根目录

* app

  * assets 静态资源文件路径

  * common 公共方法文件

  * components 公共组件文件

  * containers 页面内容文件

    demo 页面为例，下面是 demo 的目录结构

    * components demo 页面组件
    * reducers reducer 配置
    * sagas saga 配置

    - routes 子页面内容
    - services 所有 demo 子路由的公用后台 api 文件
    - get-component.js 在 route.js 里用到，页面异步加载时，会加载 demo 页面的组件，saga 和 reducer，所以里面配置了 saga 和 reducer 的地址，以及注册了 reducer 的名称
    - indes.jsx 该页面的入口
    - styles.less 该页面的样式文件

  **上述目录文件是开发者需要重点关注的目录**

  * i18n 国际化文件
  * services 后台请求 api 文件，现在基本将此类文件放于相关页面的文件夹下
  * translations 配置不同语言的翻译
  * utils 工具文件
  * app.js 入口文件, 将 react 组建挂载到页面上
  * index.html 主页，所有 jsx、less 代码被编译成一个 js 之后将会在该页面里动态插入到</body>之前
  * reducers.js 配置了全局 reducer 和其他页面的 reducer（通过异步的方式加载）
  * store.js 初始化 store
  * vendor.js 将项目中必须用到的主要 js 库挂到 window 对象上，主要目的是编译生成 vendor.js 文件，在 index.html 页面引入。（不是每个项目都有）

* build

  通过脚本 npm run build 生成，有些项目这条命令会生成 bin 文件夹，主要看配置的名字是什么，该文件部署时使用。

* docs 文档文件

* internals 存放一些配置、测试、模板文件等

  * webpack

    该目录下有好几个 webpack 文件，不同场景下使用不同 webpack 配置文件，但所有配置文件都会加载 webpack.base.babel.js 文件，至于具体在什么场景下使用什么配置文件，可在 package.json 的 script 脚本配置里找到。

* node_modules

  通过 npm install 下载的包

* pkgcmd 打包时用到的一些脚本文件

* server 中间件服务、node 服务等一些服务

* verdor js 库，没有相关 npm 包的 js 库都可放在此处

* package.json 项目用到的 npm 包 的配置，项目启动、编译等命令的脚本管理

* proxy.json 代理文件
  ​

## 项目用到的前端技术栈

* React： 用于构建用户界面的 javaScript 库，主要用于构建 UI；具体文档参考地址：[React 官方文档](https://reactjs.org/)
* React-router： 一个基于 React 之上的强大路由库，它可以让你向应用中快速地添加视图和数据流，同时保持页面与 URL 间的同步；具体文档参考地址：[React-router 中文文档](http://react-guide.github.io/react-router-cn/docs/Introduction.html)
* React-redux： 是 JavaScript 状态容器，提供可预测化的状态管理；具体文档参考地址：[React-redux 中文文档](http://www.redux.org.cn/)
* Redux-saga： 是一个 redux 的中间件， 一般是用来处理 redux 的异步操作；具体文档参考地址：[Redux-saga 中文文档](http://leonshi.com/redux-saga-in-chinese/docs/introduction/BeginnerTutorial.html)
* Ant-Design： 是一套基于 React 的高质量 UI 组件， 一般用于开发企业级后台产品；具体文档参考地址：[Ant-Design 官方文档](https://ant.design/docs/react/introduce)
* immutable： javaScript 中的对象一般是可变的（Mutable)， 因为使用了引用赋值，新的对象简单的引用了原始对象，改变新的对象将影响到原始对象，当应用复杂后，这可能会造成了非常大的隐患，immutable.js 恰好是解决这个问题的库；具体文档参考地址：[immutable 官方文档](http://facebook.github.io/immutable-js/docs/)

## 技术栈入门介绍

### react

1.  一个专注 view 层（UI 层）的框架
2.  优势

* 虚拟 DOM
* 性能很高
* 可以用到移动端

3.  劣势

* 学习成本偏高
* react 本身做的东西小，必须用 react 周边产品

  ```javascript
  let a = (
    <div>
      <div>welcome div</div>
      <span>span</span>
    </div>
  );

  let a = (
    <div>
      <div>welcome div</div>
      <span>span</span>
    </div>
  );
  ```

4.  单标记必须闭合 `<img />`
5.  jsx 语法里面写 js 代码，代码块用{}
6.  事件名采用驼峰命名法，单词首字母大写

```html
  onclick -> onClick
  onchange -> onChange
  onmouseover -> onMouseOver
  onmousemove -> onMouseMove
```

### react 开发模式

1.  直接引入文件
2.  基于 webpack

### react 语法

1.  定义组件


````html
class Title extends React.Component{
  render(){
    return <h3>welcome React</h3>
  }
	}

	调用:
	<Title />
	```
	- 参数,只读，不能修改  
```html
	<Title a ="well" />
	this.props.a
	```
	- 状态 [demo]
```html
	this.state={
		key:value
	};

	this.state.key

	状态更新、视图跟着更新:
		this.setState({
			key:value
		});

	```
	- 获取某个元素的方法 [demo]

	```html
	a). ev.target

	b). <div ref="div1"></div>
	     this.refs.div1
	c). document.querySelector()
	```
2. 组件的生命周期 [demo]

	```html
componentWillMount	组件挂载前
componentDidMount		组件挂载后
componentWillUpdate	组件更新前
componentDidUpdate	组件更新后
componentWillUnmount 组件卸载
	```
3. 组件之间通信
- 组件可以嵌套 `<App><Titlte  /></App>`
- 父子组件通信
[demo2]

```html
父级 -> 子级:
		<Child msg={这里父级数据} />

		this.props.msg
	子级 -> 父级:
		<Child fnCallBack={这里父级一个函数} />

		在子级里面执行函数:
		this.props.fnCallBack(传入子级数据)
````

4.  交互

    react 本身没有提供交互库,选择自由

5.  组件放到哪


```html
ReactDOM.render(<Title />,document.querySelector('#app'))
```

### es6

1.  新增声明变量的方法：const,let
2.  变量的解构赋值：可用于提取 json 对象中的数据`const {key1,key2} = this.props`
3.  字符串的扩展：(``)模板字符串，其中引入变量用${}
4.  箭头函数：
    * const f = （参数） => 代码块;箭头函数没有自己的 this，而是引用外层的 this
    * 如果代码块部分多余一条语句，使用 retrun {}
    * 如果返回一个对象，用大括号包起来（{}）
    * 简化回调函数：[1,2,3].map(x => x \* x)
5.  扩展运算符 ...
6.  promise 对象，获取异步操作的消息

    ```javascript
    var promise = new Promise(function(resolve,reject){
    	//...code
    	if(/*异步函数调用成功*/){
    		resolve(value) //异步操作的结果，作为参数传递出去
    	}else{
    		reject(error)
    	}
    })
    promise.then(function(){value},function(){error})
    ```

7.  Generator 函数：是一个状态机，封装了多个内部状态，执行返回一个遍历器对象,只有调用 next()方法才会遍历下一个内部状态


```html
	function* generator(){
		yeild 表达式
		yeild ..
	}
```

8.  async 函数：返回一个 promise 对象，可用 then 方法指定下一步操作

    ```html
    async function(){
    	await 表达式
    	await ..
    }
    ```

### immutable.js

将 javascript 对象转化为不可变数据类型，代替深拷贝，但不是基于递归复制，性能高很多

1.  **fromJs()**,将 javascript 的 Object,Array 转化为 Map 和 List，与 javascript Object 不同的是，Immutable Maps 的键可以是任意类型的参数
2.  **toJS()**，将 immutable 深度转换,**toJSON()**相对为浅转换，**toArray()**,转换为数组，**toObject（）**，转换为对象
3.  **get(key)**,**set(key,value)**，通过键名获取和设值方法，**getIn(keypath)**，**setIn(keypath,value)**，通过键或索引的路径获取和设置嵌套集合的值,更多方法查看 [官方 API](http://facebook.github.io/immutable-js/docs/#/)
4.  **update(key,updater)**,**updateIn()**，修改集合的值

### redux

redux 是用来解决 component -> action -> reducer -> state 的单向数据流转的问题。

redux 是一个提供可预测化状态管理的容器，主要包含 store,reducer,action 三个概念，应用中的 state 以对象树的形式存储在 Store 中，通过 dispatch action 来改变 state,描述 action 如何改变 state 的函数叫 reducer

1.  action

* action 设计规范， type,payload,error.meta
* 异步 action，通过中间件实现

2.  reducer

* reducer 必须是纯函数

3.  store

* 单一 Store
* 根据 reducer 创建 store,`createStore(reducer,initialState)`

### redux-sagas

可以把所有的业务逻辑都放在 saga 里，那么 reducer,action.component 可以很纯粹，只干他们原本要做的事情

sagas 是用于处理 redux 的异步操作的中间件，通过 genarator 函数创建，每个任务通过 yeild **Effects**完成，Effects 是 javascript 对象，包含了 sagas middleware 执行的信息

1.  创建一个中间件，将 sagas 与 redux store 建立连接

    **createSagaMiddleware(...sagas)**,sagas 为 Generator 函数列表的数组


````html
const sagaMiddleware = createSagaMiddleware(...sagas)
//sags:Array
const store = createStore({
		reducer,
		initialState,
		applyMiddleware(sagaMiddleware)
	})
	```
2. saga辅助函数
- **takeEvery(pattern,saga,...args)**,在发起的action与pattern匹配时，触发saga，允许并发

``` html
import {takeEvery} form 'redux-saga'
function* watch(){
	yeild*   takeEvery('FETCH_USER',fetchUser)
}
````

* **takeLatest(pattern,saga,...args)**,只会触发最后那次 Action,防止多次点击提交

3.  Effect 创造器

* **take(pattern)**,middleware 等到 action 匹配 pattern
* **put(action)**,异步发送 action 到 store
* **call(fn,...args)**,middleware 调用 fn(...args)，fn 为一个 generator 函数或者返回一个 promise 的普通函数
* **call([context,fn],...args)**,可指定上下文,也可用**apply(context,fn,...args)**
* **fork(fn,...args)**,无阻塞调用,返回一个 task,指定上下文**fork([context,fn],...args)**
* **cancel(task)**,取消之前的 fork 任务
* **select(selector,...args)**，通过 selector 获取 store state 的数据，如果参数为空，将获取整个 store state

### React Router

* UI 以**盒子嵌套盒子**的方式表现
* 获取 URL 参数：
  * /inbox/message/:id,`this.props.params.id`
  * /fool?bar=baz,`this.props.location.query.bar`

1.  路由配置
    * 使用`IndexRoute`设置默认页面
    * 如果使用绝对路径，用`<Redirect from='message/:id' to='/message/:id'/>`重新定位，或者用 route 数组对象配置


```html
const routeConfig = [
{
	path:'/',
	component:APP,
	indexRoute:{component:Dashboard},
	childRoutes:[
		{path:'about',component:About},
		{path:'inbox',component:Inbox,
		childRoutes:[
			{path:'/message/:id',component:Message},
			{path:'message/:id',
				onEnter:function(nextState,replaceState){
					replaceState(null,'/message/',nextState.params.id)
				}
			}
		]
	]
}
]
render(
	<Router routes = {routeConfig}/>,rootElement
)
```

2.  histories


    作用:history监听浏览器地址的变化，并解析url转化为location对象，然后router使用它匹配到路由，渲染对应的组件,推荐使用browserHistory

`<Router routes = {routes} history = {browserHistory}/>`

* 使用`browserHistory.push('some/path')`实现外部导航

3.  动态路由

    Route 可以定义`getChildRoutes`,`getIndexRoute`,`getComponents`这几个异步执行函数，React Router 会逐渐匹配 URL 并只加载对应页面所需的路径配置和组件

4.  跳转前确认

`routerWillLeave`,返回 false 取消跳转，返回提示信息，提示用户

```html
import { Lifecycle} from 'react-router',
class Home extends React.Component{
mixins:[Lifecycle],
routerWillLeave(){
  if(!this.state.isSaved){
    return 'leave?'
  }
}
}
componentDidMount() {
  this.props.router.setRouteLeaveHook(
    this.props.route,
    this.routerWillLeave
  )
}
routerWillLeave=(nextLocation) =>{
  // 返回 false 会继续停留当前页面，
  // 否则，返回一个字符串，会显示给用户，让其自己决定
};
```

5.  通过组件生命周期钩子获取数据

    ```html
    componentDidMount(){
    //获取初始化数据
    }
    componentDidMount(prevProps){
    //通过参数更新数据
    let oldId = prevProps.params.id;
    let newId = this.props.params.id;
    if(newId!=oldId){
    	this.fetchData()
    }
    }
    componentWillMount(){
    //在组件移除前忽略正在进行中的请求
    this.ignoreLastFetch = true
    }
    ```

## Demo

### 基础 demo：如何从零开始创建一个页面

* 在 app/containers 下新建 demo 文件夹
* 写 getComponent.js 文件

  ```javascript
  import errorLoading from "common/load-route-error";
  async function doGet() {
    try {
      const component = await import("./index.jsx");
      return component;
    } catch (err) {
      throw err;
    }
  }
  export default function create(options) {
    const { errorLoading, injectReducer, injectSagas } = options;
    return function(nextState, cb) {
      doGet()
        .then(component => {
          cb(null, component.default);
        })
        .catch(errorLoading);
    };
  }

  // 如有saga和reducer
  ```

* 写 index.jsx 文件

  ```javascript
   import React， { Component } from 'react';
    class Demo extends Component {
      render() {
        return (
          <div className = "app-content">
            <div>
              hello World
            </div>
          </div>
        )
      }
    }
  ```

* 在 app/route.js 文件里添加页面路由

  ```
   import Demo from './containers/demo/getComponent'
    ....
   {
      path: 'demo',
      name: '例子',
      getComponent: Demo(options)
   }
      ...
  ```

  此时打开页面 localhost:5000/demo 就能看到 demo 内容啦！

* 基础 demo 展示了如何用写一个基础的 React 组件。在 react 中，
  1.  首先在组件写法上， 我们一般通过继承的方式来创建一个组件，每个组件都有一个 render 方法，它是一个用来告诉页面 render 的标志。
  2.  其次，在语法中采用 JSX 语法，JSX 是 React 的核心组成部分，它使用 XML 标记的方式去直接声明界面，界面组件之间可以互相嵌套；具体关于 jsx 语法的介绍可参见博客:[jsx 语法介绍](http：//blog.csdn.net/a153375250/article/details/53434299)
  * 另外，写 React 组件基本注意要点如下：
    1.  要在文件头部用 import 方法引入 react 库
    2.  在 react 中通常约定组件类的第一个字母必须大写，html 标签都是小写
    3.  类名属性不能写 calss，要写成 ClassName
    4.  组件中可以嵌套其他组件(Demo 中嵌套了 Explain 组件)

### 生命周期

* 该 demo 中给出了 React 生命周期的示例图，详细关于生命周期的说明参考：
  [React 生命周期](http://www.runoob.com/react/react-component-life-cycle.html)

### 父子组件通信

* demo 目的： 演示了父子组件如何通过 props 和 state 进行通信的，以及如何使组件重新渲染的
* 父组件
  ```html
  class Father extends Component {
    constructor (props){
      super(props)
      this.state = {
        fatherTitle: 'wait child to father',
        childTitle:'我是子组件,名字是从父组件传过来的'
      }
    }
    handleClick = (fatherTitle) => {
      this.setState({
        fatherTitle
      })
    }
    render () {
      return (
        <div className = 'app-content'>
          <Card title = '父子组件通信'>
            <p>父组件title: { this.state.fatherTitle }</p>
            <Child
                handleClick = { this.handleClick }
                childTitle = { this.state.childTitle }
            />
          </Card>
        </div>
      )
    }
  }
  ```
* 子组件

  ```html
  class Child extends Component {
    render() {
      const { handleClick, childTitle } = this.props
      return (
        <Card title = '子组件'>
          <p>子组件title:{ childTitle }</p>
          <Button onClick = {() => handleClick('child to father') }>点击修改父组件title</Button>
        </Card>
      )
    }
  }
  ```

* React 中 state 是 React 中组件的一个对象.React 把用户界面当做是状态机，用户界面会随着 state 的变化而变化。而 props 是一种父级向子级传递数据的方式，一般通过父组件传入新的数据才能把新的 props 传入组件中，组件才会重新渲染
* React 父子组件之间通讯，一般可以利用 props 和 state 完成，首先 React 是单向数据流，父组件可以向子组件传递 props。实现父子组件双向数据流整体的思路是：
  1.  父组件可以向子组件传递 props，props 中带有初始化子组件的数据，还有回调函数
  2.  子组件修改父组件信息，可以在子组件的事件处理函数中，手动触发父函数传递进来的回调函数，同时时将子组件的数据传递回去

### 兄弟组件通信

* demo 目的：兄弟组件通信如果采用组件间共同的父级来进行中转，会增加子组件和父组件之间的耦合度，故在这个例子中我们需要 redux 以及 redux-saga 的支持。从组件角度看，redux 的一般使用场景： 1.某个组件的状态，需要共享 2.某个状态需要在任何地方都可以拿 3.一个组件需要改变全局状态 4.一个组件需要改变另一个组件的状态(该例子是这个场景)

* 首先来看下 redux 工作流，下面先来总体了解一下 redux 应用的基本原理，一图胜千言。
  ![sibling-eg](./imgs/flow.png)
  可见整个 redux 流程的逻辑非常清晰，数据流是单向循环的，就像一个生产的流水线：

  ```html
    store（存放状态） -> container（显示状态） -> reducer （处理动作）-> store
  ```

  我们来看看这四个概念：

  1.  store 是应用的状态管理中心，保存着是应用的状态（state），当收到状态的更新时，会触发视觉组件进行更新。
  2.  container 是视觉组件的容器，负责把传入的状态变量渲染成视觉组件，在浏览器显示出来。
  3.  reducer 是动作(action)的处理中心， 负责处理各种动作并产生新的状态（state），返回给 store。
  4.  redux-saga 是 Middleware，主要是负责改变 Store 中的 dispatch 方法，从而能处理不同类型的 action 输入。

  我们可以做个形象的比喻，把 js 比喻成巴士，把 store， container， reducer 比喻为三个车站，再把 state 和 action 比喻成两种乘客。这是一趟环路巴士：

  ```html
  js巴士 从 store车站 出发，载上 state乘客 ，state乘客 到达某个 container车站 下车并把自己
  展示出来;
  过了一会，有一个 action乘客 上车了，js巴士 把 action乘客 直接送到 reducer车站(或者先把
  action送到redux-saga中转站，经过中转站生成一个新的action，再送到reducer车站)，在这里
  action乘客 和 state乘客 生了一个孩子 new state，js巴士把 new state 送回了 store车站
  （好像是人生轮回）
  ```

* 好了，现在我们大概了解了 redux 的数据流程，那么再结合 demo 来分步讲解。这个 demo 的功能很简单，就是 组件 A 增加一个任务，然后组件 B 显示任务列表。
  * 首先： 要注意在项目中要使用 Redux 以及 redux-saga，要在 app/routes 中配置路由，而且该组件的引入方式要用 getComponent 引入
  ```js
    {
      path： 'sibing'，
      name： '兄弟通信'，
      getComponent： Sibing(options)
    }
  ```
  * 我们的 demo
    ![sibling-eg](./imgs/1.png)
  * 用户先从 A 组件中`dispatch`一个动作，动作中一定要带 type
  ```html
    handlerClick = () => {
      var value = this.state.value
      const { todo } = this.props.todo
      if(value === ''){
        message.error('任务不能为空',1.2)
        return
      } else if(_.indexOf(todo,value) !== -1) {
        message.error('该任务已经在列表中',1.2)
        return
      } else {
        this.props.dispatch({
          type:'add-todo',
          payload:{
              value
          }
        })
      }
    }
  ```
  然后 saga 监听到 `add-todo`这个 action，触发 handler 这个函数，handler 函数经过处理，再`put`一个`add/todo`动作到 reducer
  ```html
    import { handleActions，createAction } from 'redux-actions';
    import { fromJS } from 'immutable';
    const todo = ['做作业'，'做菜']
    const initialState = fromJS({
        todo
    })
    const reducer = handleActions({
        'add/todo'：(state， action) => {
            return state.updateIn(['todo']，(text) => {
                return text.push(action.payload.value)
            })
        }
    }，initialState)
    export default reducer
  ```
  reducer 接收参数 state，action，经过处理，把新增的任务放到原来的任务数组里面，再返回给 store，store 再传给组件，组件 B 把新的任务渲染出来。注意：connect()方法做到了把 React 和 redux 连接起来，是 react-redux 库提供的，用来封装组件，把 state 和 dispatch 注入到 Container 组件中
  ```html
    function mapStateToProps(state) {
      var tableData = state.get('add-todo').toJS();
      return {
          todo： state.get('add-todo').toJS()
      };
    }
    function mapDispatchToProps(dispatch) {
      return {
          dispatch
      };
    }
    export default connect(mapStateToProps， mapDispatchToProps)(Container);
  ```
* 注：关于 redux，以及 redux-saga 的用法其他参考文档有：
  2.  [redux 入门篇](https://www.jianshu.com/p/19d9f6d6fc4b)
  3.  [react-redux 原理分析](https://www.cnblogs.com/Leo_wl/p/5870010.html)
  4.  [redux-saga 管理数据流](https://segmentfault.com/a/1190000010205210)

### Ant-Design 组件

使用 Ant Design 原因:在开发企业的后台产品的时候，因为数据交互非常频繁，大量的表单、时间选择器、表格、单选多选框，以及各种标识状态的组件，如果光靠手写或者使用 jQuery 插件，其开发的工作量仍然是相当大的，而 Ant Design 是经过大量的项目实践和总结，旨在统一中台项目的前端 UI 设计的一个基于 React 开发的一个库。这样在开发中我们能够屏蔽不必要的设计差异和实现成本，解放设计和前端的研发资源。这里写了三个项目中常用到的组件:Form、Modal、Table

#### 表单

[表单详解](../part5/form.html)

#### Modal

* 基础 Modal 写了 2 个例子，一个例子是最基本的调用 antd Modal 框的写法，只要修改 Modal 的属性 visble 为 true，即可弹出一个 Modal,Modal 框中可以自定义内容，也可以放其他子组件

```html
class ModalBase extends Component {
  state = { visible: false }
  showModal = () => {
    this.setState({
      visible: true,
    });
  }
```

另外一个例子是 Modal 自带的 4 种信息提示框,调用方法如下

```html
  function info() {
    Modal.info({
      title: 'This is a notification message',
      content: (
        <div>
          <p>some messages...some messages...</p>
          <p>some messages...some messages...</p>
        </div>
      ),
      onOk() {},
    });
  }
```

信息框也可以手动移除

```html
  function info() {
    const modal = Modal.info({
        title: 'This is a notification message',
        content: (
          <div>
            <p>some messages...some messages...</p>
            <p>some messages...some messages...</p>
          </div>
        ),
        onOk() {},
    });
    setTimeout(() => modal.destroy(), 1000);
  }
```

* 复杂 Modal 是调用了一个项目封装好的一个 PopUp,`Popup` 组件，在 `Popup` 中渲染要操作的内容，完成操作过后通过传递进去的 `dispatch` 方法来实现回调，使用 Popup 的好处是 Popup 的渲染独立于当前渲染的根节点，不会对当前页面的渲染结果有任何副作用，而且只会在调用的时候才相关逻辑才会被执行


```html
  import React from "react";
  import ReactDOM from "react-dom";
  import { Modal } from "antd";
  const div = document.createElement("div");
  div.id = 'modal-content'
  document.body.appendChild(div);
  export function open(config) {
    const El = config.content;
    ReactDOM.render(
        <Modal
          visible
          title={config.title || 'ee'}
          footer={config.footer || false}
          onOk={config.onOk || null}
          onCancel={config.onCancel || null}
        >
          { config.content }
        </Modal>,
        div
    );
  }
  export function close() {
    const unmountResult = ReactDOM.unmountComponentAtNode(div);
  }
  export default {
    open,
    close
  };
```

Popup 的调用如下

```html
  import React from 'react'
  import Popup from 'components/popup';
  import ModalContent from '../components/modalContent';
  const method = 'add'
  export default function action(options) {
      Popup.open({
          title: "添加标签",
          onCancel: () => {
              Popup.close()
          },
          content: (
              <div>
                  <ModalContent { ...options } method = 'add' onClose = { ()=> { Popup.close() }} />
              </div>
          )
      })
  }
```

## 源码

[源码](https://github.com/langyazhixue/React-Family.git)

