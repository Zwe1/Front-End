# 基本使用方法（v16.0.x）
## 核心语法
* JSX : 形如HTML和Javascript的组合，是一种 JavaScript 的语法扩展。
* JSX有如同HTML一般的写法，符合你的写作习惯，表达式需要包含在大括号里。 
```
  <h1>Hello, {formatName(user)}!</h1>
```
* JSX 的特性更接近 JavaScript 而不是 HTML , 所以 React DOM 使用 camelCase 小驼峰命名 来定义属性的名称，而不是使用 HTML 的属性名称。
* JSX 允许直接在模板插入 JavaScript 变量。如果这个变量是一个数组，则会展开这个数组的所有成员。
* **虚拟DOM的特点：** 
> React is going to use virtual DOM to try to find the minimum number of steps to go from the previous render to the next. 
* React核心diff算法比较Dom树的最小化差异, 只重新渲染发生变化的Dom树局部, 提高渲染效率。
### JSX 代表 Objects
* Babel 转译器会把 JSX 转换成一个名为 React.createElement() 的方法调用。
```
  const element = (
    <h1 className="greeting">
      Hello, world!
    </h1>
  );
```
  运行效果同下：
```
  const element = React.createElement(
    'h1',
    {className: 'greeting'},
    'Hello, world!' 
  );
```
### 元素渲染
* 如上，元素事实上只是构成组件的一个部分。通过ReactDom.render()方法来将元素渲染到页面上。
* 技巧：**将界面视为一个个特定时刻的固定内容**（就像一帧一帧的动画），而不是随时处于变化之中（而不是处于变化中的一整段动画）将会有利于我们理清开发思路，减少各种bugs。
* class Demo extends React.Component {} 方法用于生成一个继承自React的衍生组件。
### 组件
* **定义**：组件从概念上看就像是函数，它可以接收任意的输入值（称之为“props”），并返回一个需要在页面上展示的React元素。 

  *定义一个组件最简单的方式是使用JavaScript函数：*
```
  function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
  }
```
* 组件类的第一个字母必须大写，只能有一个顶层标签。
* 所有组件类都必须有自己的render方法，用于输出组件。
### Props
* ***所有的React组件必须像纯函数那样使用它们的props，无法对Props进行写操作。**
* class属性需要写成className,for属性需要写成htmlFor。
* this.props 作为组件的数据入口，而 this.state 是运转于组件内部的流通数据。
* react的单项数据流，以props作为组件注入口，实现组件间的通信入口，从父组件到子组件的单向注入。  

## React组件生命周期（常用）
componentDidMount: 组件挂载完成后  
componentWillReceiveProps：组件props发生变化(深层次变化存在检测不到的清空)  
shouldComponentUpdate: 性能优化点，设置返回值为false，在不需要是阻止组件重新渲染。
## ***React 理念***
>React的众多优点之一是它让你在编写代码的时候同时也在*思考你的应用*。
#### 构建一个应用的基本思路：
1. 第一步：把 UI 划分出组件层级
>UI 和数据模型往往遵循着相同的信息架构，这意味着将 UI 划分成组件的工作往往是很容易的。只要把它划分成能准确表示你数据模型的一部分的组件就可以。
2. 第二步：用 React 创建一个静态版本
>现在有了组件层级，是时候去实现你的应用了。最简单的方式是先创建一个静态版本：传入数据模型，渲染 UI 但没有任何交互。***在较为简单的例子中，通常自顶向下更容易，而在较大的项目中，自底向上会更容易并且在你构建的时候有利于编写测试。**
3. 第三步：定义 UI 状态的最小(但完整)表示
>为了正确构建你的应用，首先你需要考虑你的应用所需要的最小可变状态集。要点是 DRY：不要重复(Don’t Repeat Yourself)。找出应用程序的绝对最小表示并计算你所需要的其他任何请求。例如，如果你正在创建一个 TODO 列表，只要保存一个包含 TODO 事项的数组；不要为计数保留一个单独的状态变量。相反，当你想要渲染 TODO 计数时，只需要使用 TODO 数组的长度就可以了。
4. 第四步：确定你的 State 应该位于哪里
>React 中的数据流是单向的，并在组件层次结构中向下传递。确定应用state的最小集合，提升至所有组件的更高父组件。
5. 第五步：添加反向数据流
>向底层组件传入所需的数据控制器，让其具备修改顶层数据的能力。

# 高级用法

1. this.props.children 属性，它表示组件的所有子节点，props.children 可以像其它属性一样传递任何数据，而不仅仅是 React 元素（字符串，数组，react元素）。

2. 组件类的PropTypes属性，用来验证组件的输入props属性是否符合要求。用于规范组件输入。(**V15.5+ 使用 prop-types 库**)
``` 
 propTypes: {
    title: React.PropTypes.string.isRequired,
  }
```  
3. 有时需要从组件获取真实 DOM 的节点，这时就要用到 ref 属性。ref可以获取到虚拟Dom对应的真实Dom节点。

4. 当组件更新时，实例仍保持一致，状态得以在渲染之间保留。React通过更新底层组件实例的props来产生新元素（字符串，react元素），并在底层实例上依次调用componentWillReceiveProps() 和 componentWillUpdate() 方法

5. React 中一个常见模式是为一个组件返回多个元素。Fragments 可以让你聚合一个子元素列表，并且不在DOM中增加额外节点。**形如:<></>**

### Portals

```
ReactDOM.createPortal(child, container)
```
Portals 提供了一种很好的将子节点渲染到父组件以外的 DOM 节点的方式。

**应用**：对于 portal 的一个典型用例是当父组件有 overflow: hidden 或 z-index 样式，但你需要子组件能够在视觉上“跳出（break out）”其容器。例如，对话框、hovercards以及提示框。(如antd中Select,Dropdown,Modal等组件的实现)

### Error Boundaries 错误边界

**目的**：用于捕获其子组件树 JavaScript 异常，记录错误并展示一个回退的UI的React组件，避免因局部错误导致整个组件树的异常。

**标志**: 如果一个类组件定义了一个名为 componentDidCatch(error, info) 的新方法，则其成为一个错误边界
```
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  componentDidCatch(error, info) {
    // Display fallback UI
    this.setState({ hasError: true });
    // You can also log the error to an error reporting service
    logErrorToMyService(error, info);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}
```
而后可以像一个普通的组件一样使用：
```
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

### 高阶组件

* 将逻辑相似或共通的组件抽离出来，从容器层向内注入差异化数据模型。高阶组件是通过将原组件包裹在容器组件里面的方式来组合使用原组件。高阶组件就是一个没有副作用的纯函数。为统一逻辑的内层组件提供数据入口，并返回一个定制化类似组件。（如Redex中的connect）

## 构建高质量代码的关键（暂定位于此）

#### 三点关键 
1. 可读性  
2. 可维护性
3. 可扩展性
#### 实现方法
1. 减小代码块，根据业务和逻辑进行抽离封装。
2. 代码复制改为代码复用
3. 分层架构