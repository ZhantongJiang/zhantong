## 一.作用域与闭包
# 函数与块儿作用域
### 函数作用域
一段语句以“function”这个为首那么就是函数声明，否则为函数表达式。  
IIFE（Immediately Invoked Function Expression）立即调用函数表达式实例：  
```
var a = 2;

(function fun () {
  var a = 3;
  console.log(a);    // 3
})()

console.log(a);   // 2
```
```
var a = 2;

(function (fun) {
  fun(window);
})(function (globle) {
  var a = 3;
  console.log(a);    // 3
  console.log(globle.a);   // 2
})

console.log(a);    // 2
```
  
### 块作用域
#### 1.var  
```
for (var i = 0; i < 2; i++) {
  console.log(i)    // 0
                    // 1
}

console.log(i)    // 2。for语句中对于i的定义是暴露在for语句所在的作用域的，此例为全局。
```
#### 2.with
with语句从对象中创建的作用域仅存在于这个with语句的生命周期中，而不在外围作用域中。
#### 3.try/catch
```
try {
  undefined();
} catch (err) {
  console.log(err);    // TypeError: undefined is not a function
}

console.log(err);   // 报异常：Uncaught ReferenceError: err is not defined
```
catch语句中的参数在全局中是访问不到的。*虽然这种行为已经被明确规定，而且对于几乎所有的标准JS环境（也许除了老IE）来说都是成立的，但是如果你在同一个作用域中有两个或多个 catch 子句，而它们又各自用相同的标识符名称声明了它们表示错误的变量时，许多 linter 依然会报警。实际上这不是重定义，因为这些变量都安全地位于块儿作用域中，但是 linter 看起来依然会恼人地抱怨这个事实。
为了避免这些不必要的警告，一些开发者将他们的 catch 变量命名为 err1，err2，等等。另一些开发者干脆关闭 linter 对重复变量名的检查。* **`暂未理解`**
#### 4.let
```
for (let i = 0; i < 2; i++) {
  console.log(i)    // 0
                    // 1
}

console.log(i)    // 报异常：Uncaught ReferenceError: i is not defined
```
实际上相当于：
```
{
  let i = 0;
  for (i; i < 2; i++) {
    console.log(i)    // 0
                      // 1
  }
}
```
#### 5.const
同let，不同的是用const声明的变量，后续再修改的时候会报错。
```
var foo = true;

if (foo) {
	var a = 2;
	const b = 3;    // 存在于包含它的`if`作用域中

	a = 3;    // 没问题！
	b = 4;    // 报异常：Uncaught TypeError: Assignment to constant variable.
}

console.log(a);   // 3
console.log(b);   // ReferenceError!
```
  
### 垃圾回收
```
function process(data) {
	// 做些有趣的事
}

var someReallyBigData = { .. };

process( someReallyBigData );

var btn = document.getElementById( "my_button" );

btn.addEventListener( "click", function click(evt){
	console.log("button clicked");
}, /*capturingPhase=*/false );
```
点击事件的处理器回调函数click根本不需要someReallyBigData变量。这意味着从理论上讲，在process(..)运行之后，这个消耗巨大内存的数据结构可以被作为垃圾回收。然而，JS引擎很可能（虽然这要看具体实现）仍会将这个结构保持一段时间，因为click函数在整个作用域上拥有一个闭包。  
块儿作用域可以解决这个问题，使引擎清楚地知道它不必再保持someReallyBigData了：
```
function process(data) {
	// 做些有趣的事
}

// 运行过后，任何定义在这个块中的东西都可以消失了
{
	let someReallyBigData = { .. };

	process( someReallyBigData );
}

var btn = document.getElementById( "my_button" );

btn.addEventListener( "click", function click(evt){
	console.log("button clicked");
}, /*capturingPhase=*/false );
```
