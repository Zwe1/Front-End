# 基础使用方法
1. JSX 允许直接在模板插入 JavaScript 变量。如果这个变量是一个数组，则会展开这个数组的所有成员。
2. React.createClass 方法用于生成一个组件类
3. 组件类的第一个字母必须大写，只能有一个顶层标签。
4. 所有组件类都必须有自己的render方法，用于输出组件。
5. class属性需要写成className,for属性需要写成htmlFor。
6.this.props.children 属性，它表示组件的所有子节点。  ——————React.Children。
7.组件类的PropTypes属性，就是用来验证组件实例的属性是否符合要求。
 propTypes: {
    title: React.PropTypes.string.isRequired,
  }
8. 虚拟DOM的意义：React is going to use virtual DOM to try to find the minimum number of steps to go from the previous render to the next.
9. 有时需要从组件获取真实 DOM 的节点，这时就要用到 ref 属性。
10. this.props 表示那些一旦定义，就不再改变的特性，而 this.state 是会随着用户互动而产生变化的特性。
11.组件生命周期——
Mounting ：已插入真实DOM
Updating：正在被重新渲染
Unmounting：已移出真实DOM	
12. 组件状态发生变化时，会触发渲染，与此前状态比较，渲染发生改变的部分。