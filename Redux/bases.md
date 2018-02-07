# Redux
## 基础
### 作用：①部署代码结构	②组件间通信
### 解决问题：
  * 用户的使用方式复杂
  * 不同身份的用户有不同的使用方式（比如普通用户和管理员）
  * 多个用户之间可以协作
  * 与服务器大量交互，或者使用了WebSocket
  * View要从多个来源获取数据

### 应用场景：
  * 某个组件的状态，需要共享
  * 某个状态需要在任何地方都可以拿到
  * 一个组件需要改变全局状态
  * 一个组件需要改变另一个组件的状态

### 设计理念：
1. Web 应用是一个状态机，视图与状态是一一对应的。
2. 所有的状态，保存在一个对象里面。
-------------------------------------------------------------
## Store

### 作用：保存数据的地方，整个应用只能有一个store。

	import { createStore } from 'redux';
	const store = createStore(fn);
	const state = store.getState();

* createStore函数接受另一个函数作为参数，返回新生成的 Store 对象。
* 当前时刻的 State，可以通过store.getState()拿到。
*	一个state对应一个view。state相同则view相同。

## Action

*	State 的变化，会导致 View 的变化。但是，用户接触不到 State，只能接触到 View。
*	所以，State 的变化必须是 View 导致的。Action 就是 View 发出的通知，表示 State 应该要发生变化了。
*	Action 是一个对象。其中的type属性是必须的，表示 Action 的名称。payload为action携带的字符串。

	const action = {
	 	 type: 'ADD_TODO',
	 	 payload: 'Learn Redux'
	};

## Action Creator

*	View 要发送多少种消息，就会有多少种 Action。定义一个函数来生成 Action，这个函数就叫 Action Creator。

	const ADD_TODO = '添加 TODO';
	function addTodo(text) {
	  return {
	    type: ADD_TODO,
	    text
	  }
	}
	const action = addTodo('Learn Redux');

## store.dispatch

*	store.dispatch()是 View 发出 Action 的唯一方法。（作用类似于Ajax的send（）函数）

	store.dispatch({
	 	type: 'ADD_TODO',
	  payload: 'Learn Redux'
	});

*	store.dispatch接受一个 Action 对象作为参数，将它发送出去。
*	结合 Action Creator，这段代码可以改写如下。
*	store.dispatch(addTodo('Learn Redux'));

## Reducer

*	Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。





