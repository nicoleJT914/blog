# 第一章 基础知识
## Chapter 1 面向对象的Javascript
### 动态类型语言
静态类型语言在编译时便已确定变量的类型，而动态类型语言的变量类型要到程序运行的时候，待变量被赋予某个值之后，才会具有某种类型。

面向接口编程，而不是面向实现编程！比如有push和pop方法，就可以看做栈。

“面向接口编程”是设计模式中最重要的思想！

### 多态
多态polymorphism：同一操作作用于不同的对象上面，可以产生不同的执行结果

多态性的表现是设计模式的目标

多态最根本的作用就是通过把过程化的条件分支语句转化为对象的多态性，从而消除这些条件分支语句。

将行为分布在各个对象中，并让这些对象各自负责自己的行为，这正是面向对象设计的优点。

### 封装
- 封装数据：

Java使用public、private等来实现

JS模拟public和private：

1. 通过ES6的let
2. 通过函数来创建作用域：
```js
var myObject = (function() {
	// private var
	var __name = 'sven'
	return {
		// public
		getName: function() {
			return __name;
		}
	}
}) ()

console.log(myObject.getName())	//sven
console.log(myObject.__name)	//undefined
```

- 封装实现：

封装使得对象内部的变化对其他对象而言是不可见的。对象对它自己的行为负责。其他对象或者用户都不关心它的内部实现。封装使得对象之间的耦合变松散，对象之间只通过暴露的API 接口来通信。当我们修改一个对象时，可以随意地修改它的内部实现，只要对外的接口没有变化，就不会影响到程序的其他功能。

- 封装类型：

封装类型是静态类型语言中一种重要的封装方式。

一般而言，封装类型是通过抽象类和接口来进行的。

把对象的真正类型隐藏在抽象类或者接口之后，相比对象的类型，客户更关心对象的行为。比如Duck和Chicken继承超类Animal

在许多静态语言的设计模式中，想方设法地去隐藏对象的类型，比如工厂方法模式、组合模式等。

- 封装变化：

封装发生变化的地方，这样才能在不重新设计的情况下进行修改。

23 种设计模式分别被划分为创建型模式、结构型模式和行为型模式。

创建型模式，要创建一个对象，是一种抽象行为，而具体创建什么对象则是可以变化的，创建型模式的目的就是封装创建对象的变化。

而结构型模式封装的是对象之间的组合关系。

行为型模式封装的是对象的行为变化。

### 原型模式

JS中根对象是Object.prototype对象，所有对象都是从Object.prototype克隆而来，Object.prototype就是所有对象的原型。

说对象的原型其实并不准确，准确来说应该是构造函数的原型。

__proto__将对象跟“构造函数的原型”联系起来，对象可以通过__proto__属性来记住它的构造函数的原型。

用原型来实现继承：
```js
var obj = {name: 'nicole'}
var A = function(){}
A.prototype = obj
var a = new A()
console.log(a.name)
```
一个“类”继承另一个“类”：
```js
var A = function(){}
A.prototype = {name: 'nicole'}
var B = function(){}
B.prototype = new A()
var b = new B()
console.log(b.name)
```

ES6的class语法
```js
class Animal {
  constructor(name) {
    this.name = name
  }
  getName() {
    return this.name
  }
}
class Dog extends Animal {
  constructor(name) {
    super(name)
  }
  speak() {
    return 'woof'
  }
}
var dog = new Dog('Scamp')
console.log(dog.getName() + ' says ' + dog.speak())
```

### 闭包的一个应用
```js
var extent = function() {
  var value = 0
  return {
    call: function(){
      value++
      console.log(value)
    }
  }
}
var extent = extent()
extent.call() //1
extent.call() //2
extent.call() //3
```
改为面向对象的形式
```js
var extent = {
  value: 0,
  call: function(){
      this.value++
      console.log(this.value)
    }
}
extent.call()
extent.call()
extent.call()
```
或构造函数+原型的形式
```js
var Extent = function(){
  this.value = 0
}
Extent.prototype.call = function() {
  this.value++
  console.log(this.value)
}
var extent = new Extent()
extent.call()
extent.call()
extent.call()
```

