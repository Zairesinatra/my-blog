---
layout: post
title:  "CommonJS-ES6Modules"
date:   2020-12-19 20:12:19 +0800
category: Tech

---

### 前言：

模块（module）体系将一个大程序拆分成互相依赖的小文件，再用简单的方法拼装起来。如果模块化不存在支持，则对开发大型的、复杂的项目形成障碍。

---

### 正文：

在没有`CommonJS`和`ES6 modules`的期间，想要实现模块化的效果存在几种方案：

- 函数作为模块：

```js
// 污染全局变量,存在模块之间冲突,且模块成员依赖混乱
function moduleA () {  
	// 模块属性与方法
}
...
```

- 对象作为模块：

```js
var moduleA = new Object({  
	_num: 0,  
	funcA: function () {},  
	funcB: function () {}  
})
// 模块成员完全暴露,状态易被无意更改
```

---

在发展过程中，由于需求的不断提出，模块的导入导出规则逐渐被建立：CommonJS/AMD/ES6 Modules。

CommonJS 常用于服务器（Node.js），AMD 用于浏览器，而 ES6 在语言标准的层面上，实现了模块功能，完全可以取代 CommonJS 和 AMD 规范，<u>成为浏览器和服务器通用的模块解决方案</u>。ES6 模块可以在<u>编译时确定模块间的依赖关系</u>，以及输入和输出的变量。而 CommonJS 和 AMD 模块，都<u>只能在运行时确定</u>。

```js
// node.js
const { normalize, join, resolve, relative } = require('path');
```

这里表示整体加载 Node.js 的内置 path 模块后，在生成对象上调用方法，也就是运行时加载。 

```js
// ES6 modules
import { normalize, join, resolve, relative } from 'path';
```

而 ES6 Modules 是从暴露模块的 `export` 命令中按需导入指定的方法，其余不加载。

同时 ES6 Modules 也避免了 UMD 模块格式。—— 判断是否支持 Node.js 模块格式（`exports` 是否存在），存在则使用 Node.js 模块格式；判断是否支持 AMD（`define` 是否存在），存在则使用 AMD 方式加载模块；都不存在，则将模块公开到全局（window或global）。

------

> ES6 Modules 模块功能主要由两个命令构成：`export`和`import`。`export`命令用于规定模块的对外接口，`import`命令用于输入其他模块提供的功能。一个模块就是一个独立的文件。该文件内部的所有变量，外部无法获取。如果希望外部能够读取模块内部的某个变量，就必须使用`export`关键字输出该变量。

```js
// 先声明后导出
let zsname = "xzy",zzname = "xxzy"
export {zsname, zzname}
```

```js
// 导出与声明一起
export let zsname = "xzy";
export let zzname = "xxzy";
```

上述两种方法等价，但是前者因可以在将导出写在脚本尾部，便于清晰观察导出的所有变量。

那么函数、类也同样适用：

```js
// 输出 zsfirst 函数
export function zsfirst () {
	console.log('zszy first export');
}
// 等价于
function zsfirst () {
	console.log('zszy first export');
}
export { zsfirst }
```

通常情况下，`export`输出的变量就是本来的名字，但是可以使用`as`关键字重命名，且可以输出多次：

```js
function zsooo () {
	console.log('ooooooooooo');
}
export { zsooo as ooozs }
```

```js
import {ooozs} from './1.mjs'
ooozs() // ooooooooooo
```

需要特别注意的是，`export`命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系：

```js
// 报错
export 1;

// 报错
var a = 1;
export a;
```

```js
// 正解
export var a = 1;

// 正解
var a = 1;
export {a};
```

`export`命令可以出现在模块的任何位置，只要处于模块顶层就可以。如果处于块级作用域内，就会报错。

`import`命令，用于加载模块文件。常接受一对大括号，里面指定要从其他模块导入的变量名。大括号里面的变量名，必须与被导入模块对外接口的名称相同。导入的变量存在只读属性，不可以重新定义。并且`import`命令具有提升效果，会提升到整个模块的头部，首先执行。

不得不提到 `export default` 命令，为模块指定默认输出。

```js
function zsfirst () {
    console.log('zszy first export');
}
export default zsfirst
```

```js
// 报错——指定默认输出的接受方法不需要大括号接收
import {zsfirst} from './1.mjs'
zsfirst()
// import {zsfirst} from './1.mjs'
//      ^^^^^^^
// SyntaxError: The requested module './1.mjs' does not provide an export named 'zsfirst'
```

```js
// 正解
import zsfirst from './1.mjs'
zsfirst()
// zszy first export
```

`export default`导出的函数在 `import` 的时候都会被重新命名。当然在导入模块的 `.js` 文件可以替换导入名，因为 `import xxx from './...'` 中的 xxx 仅作为名称而已。

```js
import couldchangename from './1.mjs'
couldchangename()
// zszy first export
```

`export.default` 仅仅只能后面跟随一个导出。

因为`export default`命令其实只是输出一个叫做`default`的变量，所以它后面不能跟变量声明语句。

```js
// 正确
var a = 1;
export default a;
// 错误
export default var a = 1;
```

在一条`import`语句中，同时输入默认方法和其他接口：

```js
import 默认导出变量名, { 常规导出变量名A, 常规导出变量名B } from 'xxx';
```

------

CommonJS 规范规定，每个模块内部，module 变量代表当前模块。这个变量是一个对象，它的 exports 属性（即 `module.exports`）是对外的接口。加载某个模块，其实是加载该模块的 `module.exports` 属性。Node.js 为每个模块提供一个 `exports` 变量，指向 `module.exports`。而 `module.exports` 优先级大于 `exports`，即在  `module.exports` 与 `exports` 同时存在仅执行前者。

```js
const get = 'zsget';
const add = 'zsadd';
const ok = 'okokok';
module.exports={
    get:get,
    add:add
}
exports.ok = 'okokok'
```

```js
const ooo = require('./CommonJSA.js')
console.log(ooo.get); // zsget
console.log(ooo.add); // zsadd
console.log(ooo.ok); // undefined
// 去除"type": "module",且不需要使用.mjs
```

```js
const get = 'zsget';
const add = 'zsadd';
const ok = 'okokok';
// module.exports={
//     get:get,
//     add:add
// }
exports.ok = 'okokok'
exports.get = 'zsget'
exports.add = 'zsadd';
```

```js
const ooo = require('./CommonJSA.js')
console.log(ooo.get); // zsget
console.log(ooo.add); // zsadd
console.log(ooo.ok); // okokok
// 去除"type": "module",且不需要使用.mjs
```

在 Node.js 模块里，真正控制模块导出的是 `module.exports`，`exports` 只是 `module.exports` 决定导出一个对象时的一个快捷方式，假如 `module.exports` 导出其它类型的数据，比如字符串、数值、函数等等，则 `exports` 的存在没有意义。

------

### 以上

本博客所有文章除特别声明外，均采用 [CC BY-SA 4.0 协议](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) ，转载请注明出处！