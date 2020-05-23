# fed-e-task-01-01

## 1. 请说出下列最终的执行结果，并解释为什么？
```javascript
var a= [];
for (var i = 0; i < 10; i++){
  a[i] = function () {
    console.log(i)
  };
}
a[6]();
```
```javascript
答：结果是10，这是由于es5中没有块级作用域的概念，for循环中的i是全局作用域中的i，循环结束后的i累加到了10
```

## 2. 请说出下列最终的执行结果，并解释为什么？
```javascript
var temp = 123;
if (true) {
  console.log(temp);
  let temp;
}
```
```javascript
答：结果会报错（ReferenceError）if循环中使用了let标识符声明temp，产生了暂时性死区。let命令不存在变量提升，声明前调用变量会报错。
暂时性死区：只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”。暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

```
## 3. 结合ES6新语法，用最简单的方式找出数组中的最小值
```javascript
var arr=[12,34,32,89,4]
```
```javascript
答：Math.min(...arr)
```
## 4. 请详细说明var,let,const三种声明变量的方式的具体差别

答：

| |var|let|const|
|-|:-:|:-:|:-:|
|是否有块概念|无|有|有|
|是否可以声明同名变量|可以|不可以|不可以|
|是否会引起变量提升|是|否|否|
|是否会引起暂时性死区|否|是|是|
|初始值是否可以为空|可以|可以|不可以|
|声明后是否可以修改|可以|可以|不可以|
|是否可以声明为null|可以|可以|不可以|

## 5. 请说出下列代码最终输出的结果，并解释为什么？

```javascript
var a =  10;
var obj = {
  a : 20,
  fn () {
    setTimeout(() => {
      console.log(this.a)
    })
  }
}
obj.fn()
```
```javascript
答：20 ,箭头函数没有自己的this，内部的this实际就是外层代码结构的this。
```
## 6. 简述Symbol类型的用途?
```javascript
答：
1. 作为对象属性名，不用担心属性名重复，隐藏一些不希望暴露的属性
2. 代替常量
3. 定义类的私有属性和方法
4. 全局Symbol注册及获取
5. 内置Symbol常量作为内部方法的标识符，可以作为自定义对象实现JS中的接口Symbol.iterator/Symbol.hasInstance/Symbol.toStringTag
```
## 7. 说说什么是浅拷贝，什么是深拷贝？
```
答：
- 浅拷贝是指把存放变量的地址值传给被赋值，最后两个变量引用了同一份地址。浅拷贝拷贝者和被烤焙者的改变会互相影响。浅拷贝只复制一层对象的属性。Object.assign是浅拷贝
- 深拷贝是指被赋值的变量开辟了另一块地址用来存放要赋值的变量的值。深拷贝拷贝者和被烤焙者的改变不会互相影响。深拷贝递归复制了所有的层级。深拷贝的几种方法：JSON.stringify和JSON.parse(只能用于简单的对象，如果包含函数，正则，时间等会有问题)，递归实现。jQuery的extend，lodash.cloneDeep()
```
## 8. 谈谈你是如何理解JS异步编程的，EventLoop是做什么的，什么事宏任务，什么是微任务？
```
答：
- JS异步编程
1. 概念：JavaScript 是单线程语言，决定于它的设计最初是用来处理浏览器网页的交互。浏览器负责解释和执行 JavaScript 的线程只有一个（所有说是单线程），即JS引擎线程。因为 javascript 单线程的原因，单线程，就意味着一个任务一个任务的执行，执行完当前任务，执行下一个任务，这样也会遇到一个问题，就比如说，要向服务端通信，加载大量数据，如果是同步执行，js主线程就得等着这个通信完成，然后才能渲染数据，为了高效率的利用cpu, 就有了同步任务和异步任务之分。js引擎执行异步代码而不用等待，是因有为有消息队列和事件循环。

2. 异步编程的几种方式：
  1）回调函数- 有回调地狱的问题
  2）Promise
  3）generator
  4）async/await

- EventLoop
JS引擎线程会维护一个执行栈，同步代码会依次加入到执行栈中依次执行并出栈。JS引擎线程遇到异步函数，会将异步函数交给相应的Webapi，而继续执行后面的任务。Webapi会在条件满足的时候，将异步对应的回调加入到消息队列中，等待执行。执行栈为空时，JS引擎线程会去取消息队列中的回调函数（如果有的话），并加入到执行栈中执行。完成后出栈，执行栈再次为空，重复上面的操作，这就是事件循环(event loop)机制。
  
- 宏任务
可以理解是每次执行栈执行的代码就是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行）,包括 setTimeout, setInterval, setImmediate, requestAnimationFrame, I/O, UI rendering（可以看到，事件队列中的每一个事件都是一个 macrotask，现在称之为宏任务队列）

- 微任务
当前task执行结束后立即执行的任务，包括process.nextTick, Promise, Object.observe, MutationObserver等

- 宏任务和微任务的执行机制
执行一个宏任务（栈中没有就从事件队列中获取），执行过程中如果遇到微任务，就将它添加到微任务的任务队列中，宏任务执行完毕后，立即执行当前微任务队列中的所有微任务（依次执行），当前任务执行完毕，开始检查渲染，然后GUI线程接管渲染，渲染完毕后，JS引擎线程继续，开始下一个宏任务（从宏任务队列中获取）

```
## 9. 将下面异步代码使用Promise改进？
```javascript
setTimeout( function () {
  var a = "hello“；
  setTimeout( function () {
    var b = "lagou";
    setTimeout( function  () {
      var c = "I ❤ U";
      console.log(a + b + c);
    },10);
  },10);
},10);
```
```javascript
答：
new Promise(resolve => {
  setTimeout(() => {
     resolve("hello")
  },10)
}).then(a => {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(a + " lagou") 
  },10)
  })
}).then(b => {
  setTimeout(() => {
    console.log(b + " I❤U")
  },10)
})
```
## 10. 请简述TypeScript与JavaScript之间的关系？
```javascript
JavaScript 是轻量级的解释性脚本语言，可嵌入到 HTML 页面中，在浏览器端执行。
而TypeScript 是JavaScript 的超集，即包含JavaScript 的所有元素，能运行JavaScript 的代码，并扩展了JavaScript 的语法。相比于JavaScript ，它还增加了静态类型、类、模块、接口和类型注解方面的功能，更易于大项目的开发。最终会编译为javascript。

```
## 11. 请谈谈你所认为的TypeScript优缺点？
```javascript
答：
- 优点
1. 能帮助开发人员更早的检测出错误并修改。
2. 增加了代码的可读性和可维护性。
3. 更易于重构
4. 更适合开发多人协同开发的大型应用。

- 缺点
1. 语言本身多了很多概念，学习成本增加。
2. 项目初期会增加开发成本（很多类型声明的代码）。
3. 不是很适合开发小型项目

```