# es6-start-up-slides
ES6 start up slides in Chinese.

:cherries: [Click Here](https://akumatus.github.io/es6-start-up-slides/)

Use `Esc` to watch the overview.


## ECMAScript 6
[wuyue](https://github.com/akumatus)

2017.4.6






## 简介





### abstract

- 2015年6月，ECMAScript6正式通过，成为国际标准
- ECMAScript 和 JavaScript
- ECMAScript6 和 ECMAScript 2015
- [compatibility table](http://kangax.github.io/compat-table/es6/)








### 工欲善其事，必先利其器





### tools

- 编译器
- 自动构建
- 代码质量工具
- 测试框架
- 编辑器/IDE





### Babel 转码器

- [REPL在线编译器](https://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact%2Cstage-2&targets=&browsers=&builtIns=false&code=)
- .babelrc 设置转码规则和插件

```
{
"presets": ["es2015", "stage-2"],
"plugins": []
}
```

- babel-cli 用于命令行转码

```
$ babel example.js -o compiled.js
$ babel-node
> (x => x * 2)(1)
2
```





### Babel 转码器

- babel-core 调用Babel的API进行转码

```javascript
var babel = require('babel-core');

babel.transform(code, options) // => { code, map, ast }

babel.transformFile("filename.js", options, function (err, result) {
result; // => { code, map, ast }
});

babel.transformFileSync(filename, options) // => { code, map, ast }
```

- Babel 默认只转换新的 JavaScript 句法（syntax）
- babel-polyfill垫片







### features
- let, const and block
- 解构 destructuring
- 模板字符串 template string
- 对象的扩展
- 函数的扩展
- symbol





### features
- iterator 和 for-of
- 类 class
- 模块 module
- promise
- 代理 proxy







## let, const and block





### let
- 声明的变量只在它所在的代码块有效
- 不存在变量提升
- 不允许重复声明
- 暂时性死区

```javascript
var tmp = 123;

if (true) {
tmp = 'abc'; // ReferenceError
let tmp;
}
```





### const
- 用于声明常量
- 一旦声明，值就不能改变
- 必须立即初始化，不能留到以后赋值

```javascript
const PI = 3.1415;

PI = 3;
// TypeError: Assignment to constant variable.

const foo;
// SyntaxError: Missing initializer in const declaration
```







## 解构 destructuring





### Array destructuring

```javascript
let [a, b, c] = [1, 2, 3];

let [foo, [[bar], baz]] = [1, [[2], 3]];

let [x, , y] = [1, 2, 3];

let [head, ...tail] = [1, 2, 3, 4];
// head = 1, tail = [2, 3, 4]

let [x, y = 'b'] = ['a']; // x='a', y='b'
```





### Object destructuring

```javascript
let { foo, bar } = { foo: "aaa", bar: "bbb" };

let { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined

var { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = {
p: [
'Hello',
{ y: 'World' }
]
};

let { p: [x, { y }] } = obj;
x // "Hello"
y // "World"
```





### String destructuring

```javascript
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"

let {length : len} = 'hello';
len // 5
```





### others

- 等号右边的值不是对象或数组，就先将其转为对象

```javascript
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true

let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError
```







##  template string
#### 模板字符串





### 模板字符串

```javascript
// 多行字符串
`In JavaScript this is
not legal.`

// 字符串中嵌入变量
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

// 调用函数
function fn() {
return "Hello World";
}

`foo ${fn()} bar`
```







## 对象的扩展





### 属性名简写

```javascript
var birth = '2000/01/01';

var Person = {
birth,

hello() { console.log(this.birth); },

get wheels () {
return this._wheels;
},

set wheels (value) {
this._wheels = value;
},

* m(){
yield 'hello world';
}

};
```





### 属性名表达式

- 允许字面量定义对象时，把表达式放在方括号内

```javascript
let propKey = 'foo';

let obj = {
[propKey]: true,
['a' + 'bc']: 123
};
```







##  函数的扩展





###  默认值

```javascript
function Point(x = 0, y = 0) {
this.x = x;
this.y = y;
}

var p = new Point();
p // { x: 0, y: 0 }
```





###  rest参数

- 用于获取函数的多余参数

```javascript
function add(...values) {
let sum = 0;

for (var val of values) {
sum += val;
}

return sum;
}

add(2, 5, 3) // 10
```





###  扩展运算符

- 将一个数组转为用逗号分隔的参数序列

```javascript
console.log(1, ...[2, 3, 4], 5)
// 1 2 3 4 5

function push(array, ...items) {
array.push(...items);
}
```





###  箭头函数
- 函数体内的this对象，就是定义时所在的对象
- 不可以当作构造函数
- 不可以使用arguments对象
- 不可以使用yield命令

```javascript
function foo() {
setTimeout(() => {
console.log('id:', this.id);
}, 100);
}

var id = 21;

foo.call({ id: 42 });
// id: 42
```







## Symbol
#### JavaScript的第七种原始类型





### Symbol
- Symbol 是一种的数据类型
- Symbol 值与其它任何值
- 可以作为对象属性的标识符使用，能

```javascript
var sym0 = Symbol();
var sym1 = Symbol("symbol1");

Symbol("symbol2") === Symbol("symbol2"); // false

var sym = new Symbol(); // TypeError
```





### 全局共享的 Symbol

- Symbol.for：创建的symbol会被放入全局注册表中
- Symbol.keyFor：获取注册表中与某个symbol关联的键

```javascript
Symbol.for("bar") === Symbol.for("bar"); // true

var globalSym = Symbol.for("foo");
Symbol.keyFor(globalSym); // "foo"
```





### Symbol 属性名遍历

- for-in、Object.keys等对象检查特性会忽略symbol键
- Object.getOwnPropertySymbols，Reflect.ownKeys

```javascript
let obj = {
[Symbol('my_key')]: 1,
enum: 2,
nonEnum: 3
};

Object.getOwnPropertyNames(obj)
// ["enum", "nonEnum"]

Object.getOwnPropertySymbols(obj)
// [Symbol(my_key)]

Reflect.ownKeys(obj)
// ["enum", "nonEnum", Symbol(my_key)]
```





### 内置Symbol

- 共有11个内置Symbol值，指向语言内部使用的方法
- Symbol.hasInstance：判断是否为该对象的实例
- Symbol.iterator：对象的默认遍历器方法

```javascript
class MyClass {
[Symbol.hasInstance](foo) {
return foo instanceof Array;
}
}

[1, 2, 3] instanceof new MyClass() // true
```







## Set 和 Map 数据结构





### Set
- 类似于数组，但是成员的值都是
- Set 结构没有键名，只有键值，
- set.size、set.has、set.add、set.delete、clear
- set.keys、 set.values 和 set.entries

```javascript
var set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

for (let item of set.entries()) {
console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```





### Map

- Object本质上是键值对的集合
- Map类似于对象，各种类型的值都可以当作键

```javascript
var m = new Map();
var o = {p: 'Hello World'};
m.set(o, 'content')
m.get(o) // "content"

let map = new Map([
['F', 'no'],
['T',  'yes'],
]);
for (let [key, value] of map.entries()) {
console.log(key, value);
}
// "F" "no"
// "T" "yes"
```







## Iterator和for-of循环





### 遍历数组

- for
```javascript
for (var index = 0; index < myArray.length; index++) {
console.log(myArray[index]);
}
```

- forEach
```javascript
myArray.forEach(function (value) {
console.log(value);
});
```

- for-in
```javascript
for (var index in myArray) {
console.log(myArray[index]);
}
```





### for-of循环

```javascript
for (var value of myArray) {
console.log(value);
}
```

- 最简洁、最直接的遍历数组元素的语法
- 避开了for-in循环的所有缺陷
- 可以正确响应break、continue和return语句
- DOM NodeList、String、Map、Set





### 迭代器 Iterator

- for-of循环首先调用集合的 Symbol.iterator 方法，紧接着返回一个新的迭代器对象
- 迭代器对象可以是任意具有.next()方法的对象
- for-of循环将重复调用这个方法,每次循环调用一次

```javascript
var zeroesForeverIterator = {
[Symbol.iterator]: function () {
return this;
},

next: function () {
return {done: false, value: 0};
}
};
```







## 生成器 Generator

[一只会说话的猫](https://people-mozilla.org/~jorendorff/demos/meow.html)





### Generator函数

- 生成器函数使用声明
- 在生成器执行过程中，遇到yield表达式
- Generator 是一个对象生成函数

```javascript
function* helloWorldGenerator() {
yield 'hello';
yield 'world';
return 'ending';
}

var hw = helloWorldGenerator();
hw.next()
// { value: 'hello', done: false }
hw.next()
// { value: 'world', done: false }
hw.next()
// { value: 'ending', done: true }
hw.next()
// { value: undefined, done: true }
```





### Iterator?

- 把Generator赋值给对象的Symbol.iterator属性，从而使得该对象具有Iterator接口

```javascript
var myIterable = {};

myIterable[Symbol.iterator] = function* () {
yield 1;
yield 2;
yield 3;
};

[...myIterable] // [1, 2, 3]
```





### for-of遍历

```javascript
function *foo() {
yield 1;
yield 2;
yield 3;
yield 4;
yield 5;
return 6;
}

for (let v of foo()) {
console.log(v);
}
// 1 2 3 4 5
```





### next 参数

- 可以在 Generator 函数运行的不同阶段，从外部向内部注入不同的值

```javascript
function* f() {
for(var i = 0; true; i++) {
var reset = yield i;
if(reset) { i = -1; }
}
}

var g = f();

g.next() // { value: 0, done: false }
g.next() // { value: 1, done: false }
g.next(true) // { value: 0, done: false }
```





### yield*

- 在一个 Generator 里面执行另一个 Generator 函数

```javascript
function* bar() {
yield 'x';
yield* foo();
yield 'y';
}

// 等同于
function* bar() {
yield 'x';
for (let v of foo()) {
yield v;
}
yield 'y';
}

for (let v of bar()){
console.log(v);
}
// "x", "a", "b", "y"
```





#### 状态机

##### before

```javascript
var ticking = true;
var clock = function() {
if (ticking)
console.log('Tick!');
else
console.log('Tock!');
ticking = !ticking;
}
```

##### after

```javascript
var clock = function*() {
while (true) {
console.log('Tick!');
yield;
console.log('Tock!');
yield;
}
};
```





### 协程

- 第一步，协程A开始执行。
- 第二步，协程A执行到一半，进入暂停，执行权转移到协程B。
- 第三步，（一段时间后）协程B交还执行权。
- 第四步，协程A恢复执行。





### 异步编程

```javascript
function async () {
return new Promise(function(resolve, reject) {
setTimeout(() => {
resolve('ok');
}, 2000);
});
}

function* gen(){
var result = yield async();
console.log(result);
}

var g = gen();
var result = g.next();

result.value.then(function(data){
g.next(data);
});
```







## Class




### before
```javascript
function Point(x, y) {
this.x = x;
this.y = y;
}

Point.prototype.toString = function () {
return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```





### After

```javascript
class Point {
constructor(x, y) {
this.x = x;
this.y = y;
}

toString() {
return '(' + this.x + ', ' + this.y + ')';
}
}

typeof Point // "function"
Point === Point.prototype.constructor // true

Object.keys(Point.prototype)// []
```





### 继承 Subclassing

- Class之间可以通过extends关键字实现继承
- super 属性直接从子类的原型开始查找属性

```javascript
class ColorPoint extends Point {}

class ColorPoint extends Point {
constructor(x, y, color) {
super(x, y); // 调用父类的constructor(x, y)
this.color = color;
}

toString() {
return this.color + ' ' + super.toString(); // 调用父类的toString()
}
}
```





### getter, setter

- 设置存值函数和取值函数

```
class MyClass {
constructor() {
// ...
}
get prop() {
return 'getter';
}
set prop(value) {
console.log('setter: '+value);
}
}

let inst = new MyClass();
inst.prop = 123; // setter: 123
inst.prop// 'getter'
```





### Generator

```javascript
class Foo {
constructor(...args) {
this.args = args;
}
* [Symbol.iterator]() {
for (let arg of this.args) {
yield arg;
}
}
}

for (let x of new Foo('hello', 'world')) {
console.log(x);
}
// hello
// world
```





### 静态方法

- static关键字表示该方法不会被实例继承
- 父类的静态方法，可以被子类继承

```javascript
class Foo {
static classMethod() {
return 'hello';
}
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```







## Module
#### 取代 CommonJS 和 AMD 通用的模块解决方案





### export

```javascript
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;
function multiply(x, y) {
return x * y;
};

export {
firstName,
lastName,
year,
multiply as multi
};
```





### import

- 变量名必须与被导入模块对外接口的名称相同
- import命令具有提升效果
- 静态执行，不能使用表达式和变量

```javascript
import {firstName, lastName, year} from './profile';

import { lastName as surname } from './profile';

import * as circle from './circle';

// 报错
if (x === 1) {
import { foo } from 'module1';
} else {
import { foo } from 'module2';
}

import 'lodash';
```





### export default

- 为模块指定默认输出，无需知道变量名或函数名
- export default命令只能使用一次

```javascript
export default function () {
console.log('foo');
}

import customName from './export-default';
customName(); // 'foo'

// 对比
export function foo() {
console.log('foo');
};

import {foo} from './export';
```





### 复合写法

- 先输入后输出同一个模块

```javascript
export { foo, bar } from 'my_module';

// 等同于
import { foo, bar } from 'my_module';
export { foo, bar };

// 接口改名
export { foo as myFoo } from 'my_module';

// 整体输出
export * from 'my_module';

// 默认接口
export { default } from 'foo';
```







## Promise





### example

```javascript
var promise = new Promise(function(resolve, reject) {
// ... some code

if (/* 异步操作成功 */){
resolve(value);
} else {
reject(error);
}
});

promise.then(function(value) {
// success
}, function(error) {
// failure
});
```







## Proxy





## example

- 在目标对象之前架设一层“拦截”

```javascript
var obj = new Proxy({}, {
get: function (target, key, receiver) {
console.log(`getting ${key}!`);
return Reflect.get(target, key, receiver);
},
set: function (target, key, value, receiver) {
console.log(`setting ${key}!`);
return Reflect.set(target, key, value, receiver);
}
});

obj.count = 1
//  setting count!
++obj.count
//  getting count!
//  setting count!
//  2
```









## what's more ?

- reflect
- Decorator
- async
- ...







## 立刻开始编写ES6吧！
#### [编程风格](https://github.com/airbnb/javascript)







## Thanks!
### FAQ
    

    

    
    
    


