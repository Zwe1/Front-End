# Javascript中的魔鬼  

## 写作意图

这篇文章用于总结一些javascript语言中常见的易混淆点。

### Call Apply Bind

在js中，最诡异莫测的莫过于this了，理解的不够深入或是应用场景略微复杂，使用时就会出现各种意想不到的错误。所以，在很多时候，我们需要手动指定上下文环境，来修正this的指向。  
最简单判断this所在环境的方法是，寻找this的实际调用者。  

**一个典型的错误**  
```
    var a = {
        name: 'ein',
        sayName: function () {
            console.log(this.name);
        }
    }

    var b = a.sayName;
    b();  //undefined
```
我们本想将对象a的sayName赋值给变量b，通过调用来查看a的name是什么？结果却输出了undefined。发生这个异常的原因就是因为在调用函数b时，sayName中的this已经不再指向对象a而是指向了全局对象window，由于window下并没有name属性，所以输出undefined。  

下面我们将通过以上三种方法来修正这个问题。  

**使用call**
```
    var b = a.sayName;
    b.call(a);  //ein
```
使用call方法，第一个参数为函数调用时的上下文环境，将其设定为对象a，这样this就会指向对象a,如此便可以取到对象a中的name属性，输出想要的值。  

**使用apply**
```
    var b = a.sayName;
    b.apply(a); //ein
```
使用apply方法，传入上下文环境，可以实现同样的效果。  

**call和apply的区别**  
那么call和apply的区别是什么呢？他们的区别在于后续的参数。让我们改造一下上面的代码，来观察效果。  
```
    var a = {
        name: 'ein',
        sayName: function (fistname, lastname) {
            console.log(`${fistname} ${this.name} ${lastname}`);
        }
    }

    var b = a.sayName;

    b.call(a,'nick','snow');  //nick ein snow
    b.apply(a,['nick','snow']);  //nick ein snow
```
call和apply的区别在于后续参数，call依次传入后续参数，将被函数所使用。apply需要将后续参数以数组的形式传入。  

**使用bind**  
bind同样可以用来修正this的指向，它与以上二者的区别在于，bind绑定之后并不会立即执行函数。  
```
    var b = a.sayName;
    b.bind(a);
```
在为b绑定a的上下文环境之后，b并不会立即执行。  
```
    b.bind(a)();  //ein
    var c = b.bind(a);
    c();  //ein
```
如此，便可以如愿得到你想要的效果了。使用bind方法可以延缓函数的执行时间，在你想调用时再执行函数。  
bind方法同样可以传入其它参数，和call方法的传入方式相同。  

### splice slice split  

看到这三个方法有没有头晕目眩的感觉？因为它们三个实在是太像三胞胎了，真的很难区分。首先，我们来学习或是回顾一下这三个单词。
```
splice  拼接  
slice   片，切片  
split   分裂，裂开
```
大多数api其实其名称都与其用途有所关联，这三个api便是很经典的案例。  
* splice意为拼接，它的用途，便是将一个数组分割开，并且可以再以指定的方式重新拼接在一起。  
* slice意为切片，我们可以使用它来在一个数组或是字符串中切取我们想要的一段。  
* split意为分裂，它可以将一个字符串分裂成一个数组。  

**使用splice**  
```
    var a = [1,2,3,4,5];
    a.splice(1,2,4,4);
    console.log(a);
    //[1,4,4,4,5]
```  
splice(starts, count, item1, ..., itemx)方法接受多个参数,第一个参数为删除的起始元素，第二个参数为删除数量，后续为插入的内容。  
**注意**: splice方法会修改原始数组，返回被删除的内容数组。  

**使用slice**
```
    var a = [1,2,3,4,5];
    var b = '12345';
    a.slice(1,3);  //[2,3]
    a.slice(1);  //[2,3,4,5]
    a.slice(2,-1)  //[3,4]
    b.slice(1,2);  //'2'
    console.log(a);  //[1,2,3,4,5]
    console.log(b);  //'12345'
```
* slice方法既可应用于数组也可应用于字符串。  
* slice(stats, beforeEnds)方法接受最多两个参数,第一个参数代表的序列号必须小于第二个参数，第二个参数为切片的终止位置，第二个参数省略时默认截取到数据末尾，参数为负数时，将反向查找匹配项。  
* **注意**: slice方法不会修改原始数组，返回的是被切片节选的片段。  

**使用split**  
```
    var a = '123456';
    a.split('');  //['1','2','3','4','5','6']
    a.split('',3);  //['1','2','3']
    console.log(a);  //'123456'
```  
split(separator, count)方法可接受两个参数，第一个参数为分割符，用于指定字符串的分割规则，第二个参数为返回数组的最大长度，返回的输出长度不会大于这个参数。  
**注意**: split不会修改原始字符串，返回值为新数组。

