# saga+redux+container数据流转

## 三大概念的简介
- saga:概括的来说，saga是为了处理redux的异步操作(Side Effects)，这里的异步操作可以理解为redux里的异步action。saga一般都是通过[Generator](http://leonshi.com/redux-saga-in-chinese/docs/ExternalResources.html)来创建的。
- redux：管理react应用的state.
- container:在这里可以理解为容器组件（redux中一般将组件分为容器组件和展示组件）。

## 数据流转过程(以Demo_简单计时器为例)
- 首先，react应用的initialState来源于reducer,在container（容器组件，通常为index.js or index.jsx）中通过`react-redux`库提供的`connect()`来包装展示组件，将state,dispatch注入到展示组件中。如下，Timer为展示组件。
	```javascript
	const timer = connect(
    	mapStateToProps
	)(Timer)
	```
- 经过包装的展示组件获得了初始的数据和dispatch action的能力。这里的action可以简单的区分为同步action和异步action,同步的action可以直接交由reducer来处理，异步的action就要用到saga来处理。

	在实际的开发中是在action到达store的过程中加入一层中间件来处理action,之后交给reducer来处理action，从而返回新的state来触发rerender.
	在该Demo中，展示组件中dispatch的全都是同步action（STOP,RESET,START）,计时的动作(TICK)是要周期性的进行dispatch，在这里就可以理解为异步action。
	```html
	<button
       disabled={this.props.status === 'Running'}
       onClick={() => this.props.dispatch(actions.reset())}
    >
        Reset
    </button>
    <button
        disabled={this.props.status === 'Running'}
        onClick={() => this.props.dispatch(actions.start())}
    >
        Start
     </button>
    <button
        disabled={this.props.status === 'Stopped'}
        onClick={() => this.props.dispatch(actions.stop())}
    >
        Stop
    </button>
	```
- saga 中采用的是Generator函数。为了dispatch异步的action(在saga的概念里，应该称作Effects)，在实际开发中，需要采用`redux-saga/effects`库。该库中封装了很多`Effects Creators`。在该Demo中我们需要用到`call()`和`put()`方法。
	- `call()`:用来声明式的调用异步函数，在本例中调用一个返回Promise对象的函数
	- `put()`:用来发送一个action到redux store中。可以简单的理解为saga中dispatch action的方式。

	```javascript
	while (true) {
      const {stopped,tick} = yield race({ //在两个Effects之间执行race
        stopped: take("STOP"),
        tick: call(wait, 1000) //对于返回promise对象的函数采用声明式调用：：call()
      });
      if (!stopped) {
        yield put(actions.tick()); //采用声明式的put()方法来dispatch action
      } else {
        break;
      }
    }
	```
	ps:更多`Effects Creators`参见[这里](http://leonshi.com/redux-saga-in-chinese/docs/api/index.html#effect-creators)
- 到此为止，数据来源有两个部分组成：展示组件中直接dispatch的同步action、saga中经过处理之后并put的异步action（这里为了简单，所有的action都只有一个`type`字段）。两部分数据之后全部交给reducer来处理action，如下：
	```javascript
	const timer = (state = {status: "Stopped",seconds: 0}, action) => {
    switch(action.type){
        case 'START':
            return { ...state, status: 'Running' }
        case 'STOP':
            return { ...state, status: 'Stopped' }
        case 'TICK':
            return { ...state, seconds: state.seconds + 1 }
        case 'RESET':
            return { ...state, seconds: 0 }
        default:
            return state
        }
    }
    export default timer
	```
- 经过reducer的处理，返回了新的state树，并且交给了Store,由于Store注册了监听，新的state树将被用于更新View.

## example
- 以Table组件的搜索功能为例：
一个路由下的index.jsx通过了`connect`方法的包装之后，通过props将dispatch传递给子组件。在子组件中就可以在handleSearch的时候dispatch一个包含搜索keyword
的Action。
    ```javascript
    this.props.dispatch({
      type:"pm-pool/on-search",
      payload:{
        condition:keyword,
        resPoolId:this.props.ResPoolID
      }
    });
    ```
    然后该Action被saga中的watcher截获，进入相应该Action的函数。在该函数中通过put方法向redux store 中发送action，并且重新请求带有keywords的数据

    ```javascript
    yield put({
    		type:"pm-pool/set-search-condition",
    		payload:condition
    })
  	yield call(loadStorage,pageNum,pageSize,condition,resPoolId)
    ```
    最后，发送的action通过reducer的处理，将请求返回的结果放入state,并且更新table的loading状态。
    ```javascript
    const reducer = handleActions({
       //...createTableReducer("pool/manager", "res"),
       "pm-pool/set-page-loading":(state,action)=>{
          return state.setIn(["loading"],action.payload);
       },
       "pm-pool/set-page-data":(state,action)=>{
          return state.setIn(["pageData"],action.payload);
       },
       "pm-pool/set-search-condition":(state,action)=>{
          return state.setIn(["condition"],action.payload);
       },
    }, initialState);
    ```
