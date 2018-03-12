## Javascript——原型继承是如何实现的

在网页上我们随处可见说javascript是一种原型继承。然而javascript仅仅默认地在使用new操作符的这种特殊情况下提供了原型继承。因此，许多js的解读都让人感到很迷惑。
这篇文章用于阐述什么是原型继承以及如何在js中使用原型继承。  

***原型继承的目的：在 js 中实现一种对象之间的资源（对象属性）共享。***

### 原型继承的定义

关于js的原型继承，你常会看到如下定义：

>当访问一个对象的属性时，js会沿着原型链向上遍历直到寻找到匹配的属性。

大多数js实现使用 _proto_ 属性来代表原型链中的下一层对象。在这篇文章中我们会看到 _proto_ 和 prototype 究竟有什么不同之处。

>注意： _proto_ 是一种非标准表示方式，你不应该将其用在你的代码中。在这篇文章中我们用它来描述js继承是如何实现的。

下面代码显示了js 引擎如何检索属性（读取）。  
```
function getProperty(obj, prop) {
  if (obj.hasOwnProperty(prop))
    return obj[prop]
 
  else if (obj.__proto__ !== null)
    return getProperty(obj.__proto__, prop)
 
  else
    return undefined
}
```

让我们来看一个经典的案例： 一个2D点元素。一个点有两个坐标 x 和 y 以及一个方法 print。  

使用之前我们所写的原型继承方法，我们将创建一个 Point 对象以及它的三个属性： x, y, 和 point。要创建一个新的点，我们只需要使用设置在 Point对象上的 _proto_ 来创建一个新的对象。  
```
var Point = {
  x: 0,
  y: 0,
  print: function () { console.log(this.x, this.y); }
};
 
var p = {x: 10, y: 20, __proto__: Point};
p.print(); // 10 20
```

### Javascript奇怪的原型继承  
让人迷惑的是每个教授js原型继承的人都给了这样的定义却没有给如上的代码。他们所给的代码像下面这样：
```
function Point(x, y) {
  this.x = x;
  this.y = y;
}
Point.prototype = {
  print: function () { console.log(this.x, this.y); }
};
 
var p = new Point(10, 20);
p.print(); // 10 20
```

这两份代码完全不同。上面的代码中 Point 是一个函数，我们添加了一个 prototype 属性，使用了 new 操作符。这是什么鬼东西？

### new 是如何工作的
Brendan Eich 想要将 Javascript 设计成一种传统的面向对象的编程语言，如同 Java h和 C++ 那样。在这些语言中，我们使用 new 操作符来创建一个类的实例。所以他为 Javascript 添加了 new 操作符。  

* C++ 有构造函数( constructor )的概念，用于初始化实例属性。因此，new 操作符必须的作用对象必须是一个函数。  
* 我们必须把对象的方法放在某一处。由于我们使用的是一种原型语言，让我们把它置于函数的原型属性中。  
new 操作符创建一个带参数的 F 函数: **new F(arguments...)**。通过如下三步来实现：  
1. **创建类的实例**: 这是一个将 _proto_ 属性设置在 F.prototype 上的空对象。  
2. **初始化实例**： F函数调用时接受传入的参数并且将 this 指向这个实例。  
3. **返回实例**  
现在我们理解了 new 操作符是怎样工作的，我们可以在 js 中实现它。
```
function New (f) {
    ①  var n = { '__proto__': f.prototype };
       return function () {
    ②    f.apply(n, arguments);
    ③    return n;
       };
     }
```  
我们来测试一下它是如何工作的：  
```
function Point(x, y) {
  this.x = x;
  this.y = y;
}
Point.prototype = {
  print: function () { console.log(this.x, this.y); }
};
 
var p1 = new Point(10, 20);
p1.print(); // 10 20
console.log(p1 instanceof Point); // true
 
var p2 = New (Point)(10, 20);
p2.print(); // 10 20
console.log(p2 instanceof Point); // true
```  
### js中真正的原型继承  
js规范仅为我们提供了 new 操作符可以使用。Douglas Crockford 找到了一种利用 new 操作符来实现原型继承的方法！他创造了 Object.create 函数。  
```
Object.create = function (parent) {
  function F() {}
  F.prototype = parent;
  return new F();
};
```  
这种写法看起来很奇怪但它的工作原理很简单。它仅仅根据设置在这个函数上的原型来创建了一个新的对象。如果我们可以使用 _proto_ 它的写法将会是这个样子：  
```
Object.create = function (parent) {
  return { '__proto__': parent };
};
```
下面的代码是我们使用真正的原型继承来创建的一个点。
```
var Point = {
  x: 0,
  y: 0,
  print: function () { console.log(this.x, this.y); }
};
 
var p = Object.create(Point);
p.x = 10;
p.y = 20;
p.print(); // 10 20
```
### 结论
我们看到了究竟什么是原型继承并且看到了 js 仅仅是用了一种特别的方法来实现它。  
然而，使用原型继承（Object.create and _proto_）也存在一些缺点：  
* **非标准**：_proto_ 不是标准化的东西甚至被弃用。原生的 Object.create 和 Douglas Crockford 的实现也不完全相同。  
* **未优化**：与 new 构造函数（原生或自定义）相比Object.create 还没有被深度严格优化, 后者甚至慢十倍。  

### 另外  
如果你能够通过下面的图理解原型继承是如何工作的，Congratulations！  
<img src="http://i.imgur.com/cCzkv.png"/>   
  
<h3><a href="http://blog.vjeux.com/2011/javascript/how-prototypal-inheritance-really-works.html">Vjeux英文原文链接</a></h3>
