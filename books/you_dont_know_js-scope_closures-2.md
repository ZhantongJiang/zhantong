## 一.作用域与闭包
# 词法作用域？
词法作用域是在编写代码的时候就确定的。
  

### 欺骗词法作用域的两种方式
#### 1.eval()
eval()相当于修改了调用eval的那个作用域。  
```
function fun (str) {
  eval(str)
  console.log(a)
}

fun('var a = 2')    // 2
console.log(a)    // Uncaught ReferenceError: a is not defined
```
但在严格模式下，在eval()内部声明的变量不会实际上修改它的作用域。  
```
function fun (str) {
  "use strict"
  eval(str)
  console.log(a)
}

fun('var a = 2')    // ReferenceError: a is not defined
```
#### 2.with()
with()相当于开辟了一个新的作用域，是LHS的引用。  
```
function foo (obj) {
	with (obj) {
		a = 2
	}
}

var o1 = {
	a: 3
}

var o2 = {
	b: 3
}

foo(o1)
console.log(o1.a)   // 2

foo(o2)
console.log( o2.a )   // undefined
console.log( a )    // 2，全局作用域
```  
传入o1时，对o1对象进行查找，发现a属性后将值2赋予给o1.a；传入o2时，对o2对象进行查找，由于o2没有a这个属性，在非严格模式下LHS会在**当前函数调用的作用域**上新建一个变量a。
  
### 性能
JavaScript引擎在编译代码的阶段会进行类似于变量提升等的优化，但在编译过程中遇到了eval()或with()这样的字段，编译器不确定其中的内容会是什么，就不得不假定自己知道的所有的标识符的位置可能是无效的，因此所有的优化都将失去意义。  
with()已被抛弃，eval()也应避免使用。
