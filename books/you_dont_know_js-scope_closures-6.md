## 一.作用域与闭包
# 附录
### 动态作用域
JavaScript 实际上**没有**动态作用域。它拥有词法作用域。但是 this 机制有些像动态作用域。

关键的差异：词法作用域是编写时的，而动态作用域（和 this）是运行时的。词法作用域关心的是*函数在何处被声明*，但是动态作用域关心的是*函数从何处被调用*。
```
function foo () {
	console.log(a);
}

function bar () {
	var a = 3;
	foo();
}

var a = 2;
bar(); // 2。词法作用域，foo()调用时进行了RHS，由于当前的作用域并不存在，故找到了全局的a。
```

### 填补块儿作用域
try、catch的结构拥有块级结构，在前es6可模拟块级结构
```
{
	let a = 2;
	console.log(a); // 2
}

console.log(a); // ReferenceError
```
```
try {
  throw 2
} catch (a) {
	console.log(a); // 2
}

console.log(a); // ReferenceError
```
