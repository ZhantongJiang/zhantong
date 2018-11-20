## 二.this与对象原型
# this是什么
1. this并不一定指向函数自己
```
function foo (num) {
	console.log( "foo: " + num );
	// 追踪 `foo` 被调用了多少次
	this.count++;
}

foo.count = 0;

var i;
var count = 0;

for (i = 0; i < 10; i++) {
	if (i > 5) {
		foo( i );
	}
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// `foo` 被调用了多少次？
console.log(foo.count); // 0
console.log(count); // 4
```
2. this不会以任何方式指向函数的**词法作用域**
```
function foo () {
	var a = 2;
	console.log(1)
	this.bar();
}

function bar () {
	console.log(2)
	console.log(this.a);
}

foo(); //undefined。找的是全局作用域中的a，若全局定义了变量a并赋予a值的话，则执行foo()方法到执行foo()方法中的bar()方法，打印出的应该是全局作用域中a的值。
```
