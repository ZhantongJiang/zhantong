## 一.作用域与闭包
# 提升

在编译过程中，声明就会提升，并且声明的提升是以**作用域**为单位的。
```
foo();
a; // Uncaught ReferenceError: a is not defined

function foo () {
  console.log(a); // undefined
  
  var a = 2;
}
```
另外，函数声明会提升，但函数表达式并不会提升，函数会先于变量提升。
```
function foo () {
  …… // 函数声明
}

var foo = function bar () {
  …… // foo函数表达式；bar的提升是在这个块级作用域内
}
```
```
foo(); // 1

var foo;

function foo() {
	console.log( 1 );
}

foo = function() {
	console.log( 2 );
};
```
