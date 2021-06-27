---
layout: post
title:  "Comparison of discriminatinating array methods"
date:   2020-11-11 21:58:08 +0800
category: Tech

---

- **`typeof`:**

在 JavaScript 中用 `typeof` 不可以判断数组，仅有局限性的指出操作数的类型。

```js
const a = null;
const b = {};
const c= [];
const d = function(){};
const e = 123;
const f = 'zs';
const g = undefined;
const h = true;
const i = NaN;
console.log(typeof(a)); // object
console.log(typeof(b)); // object
console.log(typeof(c)); // object
console.log(typeof(d)); // function
console.log(typeof(e)); // number
console.log(typeof(f)); // string
console.log(typeof(g)); // undefined
console.log(typeof(h)); // boolean
console.log(typeof(i)); // number
```

- **`instanceof`:**

<span style="color: red">`instanceof` 运算符会测试是否存在一个构造函数的原型属性（ `prototype`）在被测试对象的原型链中出现。</span>

那么使用 `instanceof`判断一个对象是否为数组，`instanceof` 会判断这个对象的原型链（`__proto__`）上是否会找到对应的构造函数 `Array` 的原型（`prototype`）。更加深刻可以理解为 `arr.__proto__ === Array.prototype`。

忘记了原型链，可以去我的博客查看，点此[传送阵](https://blog.csdn.net/xxxzzzyyybiu/article/details/113811647?spm=1001.2014.3001.5501)。

```js
const arr = [];
const obj = {};
console.log(arr instanceof Array); // true
console.log(obj instanceof Object); // true
arr.__proto__ === Array.prototype // true
```

但是 `instanceof` 只能用来判断 引用数据类型，原始数据类型不可以。并且所有对象类型 `instanceof Object` 都是 true。

- **`Object.prototype.toString.call()`：**

每一个继承自 Object 的对象都拥有 `toString` 的方法，那么如果一个对象的 `toString` 方法没有被重写则会返回 "[object *type*]"。 重写（override）通常表示覆盖了一个方法，以实现不同的功能。通常用于子类在继承父类时，重写（重新实现）父类中的方法。

```js
// 所有数据类型都会通过相应的包装类进行包装
Object.prototype.toString.call(123); // "[object Number]"
Object.prototype.toString.call(“abc”); // "[object String]"
Object.prototype.toString.call(null); // "[object Null]"
// null与undefined没有包装类并不意味返回值没有
null.toString(); // ReferenceError
Object.prototype.toString.call(undefined); // "[object Undefined]"
undefined.toString(); // ReferenceError
Object.prototype.toString.call(true); // "[object Boolean]"
Object.prototype.toString.call(function(){}); // "[object Function]"
Object.prototype.toString.call([1,2,3]); // "[object Array]"
Object.prototype.toString.call({}); // "[object Object]"
Object.prototype.toString.call(new Error()); // "[object Error]"
Object.prototype.toString.call(/\d/); // "[object RegExp]"
Object.prototype.toString.call(Date); // "[object Function]" !!!
Object.prototype.toString.call(new Date); // "[object Date]" !!!
// Symbol() 不是一个构造器，看作函数执行唯一
Object.prototype.toString.call(Symbol()); // "[object Symbol]"
Object.prototype.toString.call(arguments); // "[object Arguments]"
Object.prototype.toString.call(document); // "[object HTMLDocument]"
```

这里注意 `toString` 为 Object 的原型方法，**而`Array` ，`function` 等类型作为 Object 的实例，都重写了 `toString` 方法**。不同的对象类型调用 `toString` 方法时，根据原型链的知识，调用的是对应的重写之后的 `toString` 方法（`function` 类型返回内容为函数体的字符串，`Array` 类型返回元素组成的字符串…..），而不会去调用 Object 上原型 `toString` 方法（返回对象的具体类型），所以采用`obj.toString()` 不能得到其对象类型，只能将 obj 转换为字符串类型；因此，在想要得到对象的具体类型时，应该调用Object上原型toString方法。

- **`Array.isArray(arr)`：**

最简单粗暴的一种方法，当参数为数组的时候，返回 true，当参数不为数组的时候，返回 false。但是 `Array.isArray()` 是ES5标准中增加的方法，部分比较老的浏览器可能会有兼容问题。

```js
const arr = [];
const obj = {};
Array.isArray(arr); // true
Array.isArray(obj); // false
```

`Array.isArray` 和 `Object.prototype.toString.call` 可以检测出 `iframes`，而 `instanceof` 不能。

- **`constructor：`**

实例对象存在 `constructor` 属性指向构造函数。但是注意 `constructor` 属性是可以改写的。

```js
const a = [];
console.log(a.constructor); // [Function: Array]
const b = {};
console.log(b.constructor); // [Function: Object]
const c = function () {};
console.log(c.constructor); // [Function: Function]
const d = /^[0-9]$/;
console.log(d.constructor); // [Function: RegExp]
```

---

### 以上

本博客所有文章除特别声明外，均采用 [CC BY-SA 4.0 协议](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) ，转载请注明出处！