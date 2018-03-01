# 基本使用方法（v16.0.x）
## 核心语法
* JSX : 形如HTML和Javascript的组合，是一种 JavaScript 的语法扩展。
* JSX有如同HTML一般的写法，符合你的写作习惯，表达式需要包含在大括号里。
    <h1>Hello, {formatName(user)}!</h1>
* JSX 的特性更接近 JavaScript 而不是 HTML , 所以 React DOM 使用 camelCase 小驼峰命名 来定义属性的名称，而不是使用 HTML 的属性名称。
* JSX 允许直接在模板插入 JavaScript 变量。如果这个变量是一个数组，则会展开这个数组的所有成员。
* **虚拟DOM的特点：** 
  React is going to use virtual DOM to try to find the minimum number of steps to go from the previous render to the next. 
* React核心diff算法比较Dom树的最小化差异, 只重新渲染发生变化的Dom树局部, 提高渲染效率。
### JSX 代表 Objects
* Babel 转译器会把 JSX 转换成一个名为 React.createElement() 的方法调用。

  const element = (
    <h1 className="greeting">
      Hello, world!
    </h1>
  );

  运行效果同下：

  const element = React.createElement(
    'h1',
    {className: 'greeting'},
    'Hello, world!' 
  );

### 元素渲染
* 如上，元素事实上只是构成组件的一个部分。通过ReactDom.render()方法来将元素渲染到页面上。
* 技巧：**将界面视为一个个特定时刻的固定内容**（就像一帧一帧的动画），而不是随时处于变化之中（而不是处于变化中的一整段动画）将会有利于我们理清开发思路，减少各种bugs。
* class Demo extends React.Component {} 方法用于生成一个继承自React的衍生组件。
### 组件
* **定义**：组件从概念上看就像是函数，它可以接收任意的输入值（称之为“props”），并返回一个需要在页面上展示的React元素。 

  *定义一个组件最简单的方式是使用JavaScript函数：*

  function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
  }

* 组件类的第一个字母必须大写，只能有一个顶层标签。
* 所有组件类都必须有自己的render方法，用于输出组件。
### Props
* ***所有的React组件必须像纯函数那样使用它们的props。无法对Props进行写操作***
* class属性需要写成className,for属性需要写成htmlFor。
* this.props 作为组件的数据入口，而 this.state 是运转于组件内部的流通数据。
  react的单项数据流，以props作为组件注入口，实现组件间的通信入口，从父组件到子组件的单向注入。
* this.props.children 属性，它表示组件的所有子节点。  
* 组件类的PropTypes属性，用来验证组件的输入props属性是否符合要求。用于规范组件输入。
 propTypes: {
    title: React.PropTypes.string.isRequired,
  }
* 有时需要从组件获取真实 DOM 的节点，这时就要用到 ref 属性。ref可以获取到虚拟Dom对应的真实Dom节点。

## React组件生命周期
Mounting ：已插入真实DOM
Updating：正在被重新渲染
Unmounting：已移出真实DOM	