---
layout: post
title:  "this keyword and usage instructions"
date:   2019-12-24 19:12:24 +0800
category: Tech

---

### 前言：

`this`是 JavaScript 的一个关键字。它是函数运行时，在函数体内部自动生成的一个对象，只能在函数体内部使用。此外，`this` 不能在执行期间被赋值。

---

### 正文：

**默认绑定：**

通常是单独或纯粹函数调用的情况下，此时 `this` 指向全局对象，在浏览器中为 `window`，而 Node.js 中为 `global`。当然，在纯粹函数调用时，由于严格模式不允许默认绑定，`this`为未定义的 `undefined`。

```js
// 单独使用;不论是否严格
console.log(this) // {}
```

```js
// 函数调用;非严格模式
global.x = "zs";
function zsall() {
	console.log(this.x);
}
zsall();  // zs
```

```js
// 函数调用;严格模式
"use strict";
function usestrictzsall() {
	console.log(this);
 }
usestrictzsall() // undefined
```

**隐式绑定：**

作为对象方法的调用场景时，通常 `this` 指向方法的调用者（通常也是'拥有者'）：

```js
// 经典题
global.name = "Kakashi";
function a () {
	var name = 'Sasuke';
	console.log(this.name);
}
function d (i) { // 形参i实际是函数
	return i() // 函数直接执行形参i
}
var b = {
	name: 'Naruto',
	detail: function() {
		console.log(this.name);
	},
	// 有括号需要执行
	// b.teacher => f(){return {function(){console.log(this.name)}}}
	// b.teacher() => f(){console.log(this.name)}
	teacher: function() {
		return function() {
			console.log(this.name);
		};
	},
}
var c = b.detail;
b.a = a; // 给b定义a属性
var e = b.teacher();
// global对象可以在全局作用域里通过使用 this 访问到
a(); // 纯粹的函数调用 global -- Kakashi
// ---
// b的第一条方法赋值给了变量c,那么真正的调用者是c
c(); // b的第一条方法看作普通函数赋值给c,已经和b没有关系了 -- Kakashi
// ---
b.a(); // Naruto
// ---
// 看着指向b,实际上是b.detail方法看成一个值用在d里面,实际执行的是全局范围的函数d,所以也是指向全局
d(b.detail) // Kakashi
e(); // 赋值了b.teacher()的输出=> var e = f(){console.log(this.name)} => 全局 -- Kakashi
```

**硬绑定：**

通常为 `apply()` 或者是 `call()` 改变函数的调用对象（将另一个对象作为参数调用对象方法）。第一个参数就表示**改变后**的调用这个函数的对象。因此，`this`指的就是这第一个参数。也就是说，将<u>所用方法指定绑定到目的对象中</u>：

```js
// 硬绑定call与apply皆可
var zswant = {
	fruit: 'coco',
	sayName: function() {
		console.log('zs想吃' + this.fruit)
	}
} 
var fruit1= {
	fruit: 'watermelon'
}
var fruit2 = {
	fruit: 'Strawberry'
}
// call调用对象的方法
zswant.sayName.call(fruit1) // zs想吃watermelon
zswant.sayName.apply(fruit2) // zs想吃Strawberry
```

注意这里：`apply()` 或 `call()` 的参数为空时，默认调用全局对象：

```js
globalThis.fruit = 'hahaha'
// 硬绑定call与apply皆可
var zswant = {
	fruit: 'coco',
	sayName: function() {
		console.log('zs想吃' + this.fruit)
	}
} 
var fruit1= {
	fruit: 'watermelon'
}
var fruit2 = {
	fruit: 'Strawberry'
}
// call调用对象的方法
zswant.sayName.call() // zs想吃watermelon
zswant.sayName.apply() // zs想吃Strawberry
```

**构造函数绑定：**

通过构造函数生成的新对象。这时，`this`就指这个新对象：

```js
function dev (name) {
	this.name = 'zs'; // 注意这里的不同
	this.sayName = function () {
		console.log('dev是' + this.name)
	}
}
var name = "zsxzy" // 即使有同名变量
var zy = new dev('zss')
zy.sayName(); // dev是zs
```

```js
function dev (name) {
	this.name = name; // 构造函数实例化传参且传入
	this.sayName = function () {
		console.log('dev是' + this.name)
	}
}
var name = "zsxzy" // 即使有同名变量
var zy = new dev('zss') // 输入进行变量实例化,这时this与实例化后新对象捆绑
zy.sayName(); // dev是zss
```

**箭头函数绑定：**

箭头函数的`this`对象，就是**外部环境所在的作用域指向的对象**，而不是使用时所在的作用域指向的对象，即并不是由自身决定（并不属于自身）的：

```js
var name = 'window'; 
var A = {
name: 'A',
sayHello: () => {
    console.log(this.name)
    }
}
A.sayHello(); // window
```

```js
var name = 'window';
var A = {
    name: 'A',
    sayHello: function(){
        return arrow = () => console.log(this.name) // 返回箭头函数
    }
}
var sayHello = A.sayHello();
sayHello(); // A 
var B = {
   name: 'B'
}
sayHello.call(B); // A
sayHello.apply(); // A
// sayHello.bind() 不会输出结果 => bind方法返回的是綁绑定 this 后的原函数
sayHello.bind()(); // A
```

箭头函数在定义时就完成了 `this` 绑定，执行时调用位置、用 `call()`、`apply()`、`bind()` 都无法更改。

---

### 结束：

总结一下：

1.`this`就是函数运行时所在的环境对象，会根据使用场合存在不同的绑定方式。此外，勿忘在严格模式和非严格模式之间的差别。

2.`call()`、`apply()`都是回调 func 执行结果，而 `bind()` 方法回调的是绑定 `this` 后的原函数。

### 以上

本博客所有文章除特别声明外，均采用 [CC BY-SA 4.0 协议](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) ，转载请注明出处！