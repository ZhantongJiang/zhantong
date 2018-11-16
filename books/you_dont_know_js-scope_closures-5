## 一.作用域与闭包
# 作用域闭包
### 闭包
*闭包就是当一个函数即使是在它的词法作用域之外被调用时，也可以记住并访问它的词法作用域。*
能够产生闭包效果的函数往往需要开辟一个新的作用域，同时将自己在当前的作用域中暴露出去，暴露出去的方法有很多种：
#### 1.return方式
```
function foo () {
  var a = 'hello';
  function inner () {
    console.log(a);
  }
  return inner;
}

var outter = foo();
outter(); // 'hello'
console.log(a); // ReferenceError: a is not defined
```
#### 2.参数传递
```
function foo () {
	var a = 2;
	function baz () {
		console.log(a); // 2
	}
	bar(baz);
}

function bar (fn) {
	fn();
}
```
#### 3.赋值
```
var fn;

function foo () {
	var a = 2;
	function baz () {
		console.log(a);
	}
	fn = baz; // 将`baz`赋值给一个全局变量
}

function bar () {
	fn();
}

foo();
bar(); // 2
```
……等等

### 循环
eg1:
```
for (var i=1; i<=5; i++) {
	setTimeout( function timer () {
		console.log(i);
	}, i*1000 );
}
```
由于setTimeout在主任务执行后才会执行，当主任务执行后i的值为6，并且由于i使用var关键字声明的，i为全局变量，执行的结果会输出五次6。
eg2:
```
function timer (i) {
	setTimeout( function timer () {
		console.log(i);
	}, i*1000 );
  
}
for (var i=1; i<=5; i++) {
  timer(i);
}
```
timer函数开辟了一个新的作用域，i作为参数存在于timer函数作用域中，在循环过程中tiemr会记录每次传来的值。同理的方式还有：
```
for (var i=1; i<=5; i++) {
	(function(j){
		setTimeout( function timer () {
			console.log(j);
		}, j*1000 );
	})( i );
}
```
```
for (let i=1; i<=5; i++) {
	setTimeout( function timer () {
		console.log(i);
	}, i*1000 );
}
```
  
### 模块
模块要求两个关键性质：
1. 一个被调用的外部包装函数，来创建外围作用域。
2. 这个包装函数的返回值必须包含至少一个内部函数的引用，这个函数才拥有包装函数内部作用域的闭包。
（具体事例参考[你不懂js中文官网](https://github.com/getify/You-Dont-Know-JS/blob/1ed-zh-CN/scope%20%26%20closures/ch5.md)）
