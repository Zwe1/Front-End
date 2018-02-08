# Promise异步编程

## Promise的含义:

  异步编程的解决方案，无需再写复杂的嵌套回调函数。清晰的链式语法，更加简洁美观。

### 特点：

1. Promise对象有三个状态：peding(进行中)、fulfilled(已成功)、rejected(已失败)。
2. Promise的状态变化只有两种可能：从peding变为fulfilled或从peding变为rejected。  
变化产生后会凝固，这时被称为resolved(已定型)。

## 基本使用：	

	new Promise((resolve, reject) => {  
	  resolve(1);  
	  console.log(2);  
	}).then(r => {  
	  console.log(r);  
	});  
	// 2  
	// 1  

**注意**：立即 resolv 或 reject的 Promise 是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。

	getJSON("/post/1.json").then(function(post) {
	  return getJSON(post.commentURL);
	}).then(function funcA(comments) {
	  console.log("resolved: ", comments);
	}, function funcB(err){
	  console.log("rejected: ", err);
	});

  Promise对象接受一个 function 作为参数，function的两个参数为任务执行成功时的回调 resolve 和任务执行失败时的回调 reject，resolve和reject函数由js引擎提供，无需自己部署，resolve和reject方法可以接受参数，可以通过此方法结构向外部环境抛出数据和信息。

## Promise.prototype.then()

  then()方法是定义在Promise对象原型上的，用于为Promise实例添加状态改变时的回调。  
  Promise实例生成后，可以用then方法接收前面Promises实例的返回状态，then 方法的两个参数为 resolve 时执行的回调和 reject时执行的回调（可选项），then方法返回的是一个新的Promise对象，所以可以采用链式写法，then会根据顺序，依次执行。

## Promise.prototype.catch()

  Promise.prototype.catch()方法相当于.then(null，rejection)。  
  resolve回调可以接受promise中的数据，reject回调接收一个 Error 对象实例。或在链式调用的最后通过 catch 方法来接收错误信息。  
  Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。错误总是会被下一个catch语句捕获。  
  catch方法可以捕获之前所有then执行中的错误。  

## Promise.all()

  此方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。  

	const p1 = new Promise((resolve, reject) => {
	  resolve('hello');
	})
	  .then(result => result)
	  .catch(e => e);

	const p2 = new Promise((resolve, reject) => {
	  throw new Error('报错了');
	})
	  .then(result => result)
	  .catch(e => e);

	Promise.all([p1, p2])
	  .then(result => console.log(result))
	  .catch(e => console.log(e));
	// ["hello", Error: 报错了]


  上面代码中，p1会resolved，p2首先会rejected，但是p2有自己的catch方法，该方法返回的是一个新的 Promise 实例，p2指向的实际上是这个实例。  
  该实例执行完catch方法后，也会变成resolved，导致Promise.all()方法参数里面的两个实例都会resolved，因此会调用then方法指定的回调函数，  
  而不会调用catch方法指定的回调函数。如果p2没有自己的catch方法，就会调用Promise.all()的catch方法。  

## Promise.prototype.finally()

finally方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。


