# ES6/ES7

由于 Babel 的强大和普及，现在 ES6/ES7 基本上已经是现代化开发的必备了。通过新的语法糖，能让代码整体更为简洁和易读。

- [变量的声明](#1-变量的声明)
- [函数的新特性](#2-ES6函数的新特性)
- [实现不变性](#3-实现不变性)

## 1. 变量的声明

### Deconstruction: 解构赋值

解构赋值允许你使用类似数组或对象字面量的语法将数组和对象的属性赋给各种变量。这种赋值语法极度简洁，同时还比传统的属性访问方法更为清晰。传统的访问数组前三个元素的方式为：

```js
var first = someArray[0];
var second = someArray[1];
var third = someArray[2];
```

而通过解构赋值的特性，可以变为：

```js
var [first, second, third] = someArray;
```

```js
// === Arrays

var [a, b] = [1, 2];
console.log(a, b);
//=> 1 2


// Use from functions, only select from pattern
var foo = () => {
  return [1, 2, 3];
};

var [a, b] = foo();
console.log(a, b);
// => 1 2


// Omit certain values
var [a, , b] = [1, 2, 3];
console.log(a, b);
// => 1 3


// Combine with spread/rest operator (accumulates the rest of the values)
var [a, ...b] = [1, 2, 3];
console.log(a, b);
// => 1 [ 2, 3 ]


// Fail-safe.
var [, , , a, b] = [1, 2, 3];
console.log(a, b);
// => undefined undefined


// Swap variables easily without temp
var a = 1, b = 2;
[b, a] = [a, b];
console.log(a, b);
// => 2 1


// Advance deep arrays
var [a, [b, [c, d]]] = [1, [2, [[[3, 4], 5], 6]]];
console.log("a:", a, "b:", b, "c:", c, "d:", d);
// => a: 1 b: 2 c: [ [ 3, 4 ], 5 ] d: 6


// === Objects

var {user: x} = {user: 5};
console.log(x);
// => 5

let title = "ES6"
let price = 20
let publish = "出版社"
 
let book = {
    title,price,publish,
    toString() {
        console.log(`<<${this.title}>> is ${price}元。`);
    }
};


// Fail-safe
var {user: x} = {user2: 5};
console.log(x);
// => undefined


// More values
var {prop: x, prop2: y} = {prop: 5, prop2: 10};
console.log(x, y);
// => 5 10

// Short-hand syntax
var { prop, prop2} = {prop: 5, prop2: 10};
console.log(prop, prop2);
// => 5 10

// Equal to:
var { prop: prop, prop2: prop2} = {prop: 5, prop2: 10};
console.log(prop, prop2);
// => 5 10

// Oops: This doesn't work:
var a, b;
{ a, b } = {a: 1, b: 2};

// But this does work
var a, b;
({ a, b } = {a: 1, b: 2});
console.log(a, b);
// => 1 2

// This due to the grammar in JS.
// Starting with { implies a block scope, not an object literal.
// () converts to an expression.

// From Harmony Wiki:
// Note that object literals cannot appear in
// statement positions, so a plain object
// destructuring assignment statement
//  { x } = y must be parenthesized either
// as ({ x } = y) or ({ x }) = y.

// Combine objects and arrays
var {prop: x, prop2: [, y]} = {prop: 5, prop2: [10, 100]};
console.log(x, y);
// => 5 100


// Deep objects
var {
  prop: x,
  prop2: {
    prop2: {
      nested: [ , , b]
    }
  }
} = { prop: "Hello", prop2: { prop2: { nested: ["a", "b", "c"]}}};
console.log(x, b);
// => Hello c


// === Combining all to make fun happen

// All well and good, can we do more? Yes!
// Using as method parameters
var foo = function ({prop: x}) {
  console.log(x);
};

foo({invalid: 1});
foo({prop: 1});
// => undefined
// => 1


// Can also use with the advanced example
var foo = function ({
  prop: x,
  prop2: {
    prop2: {
      nested: b
    }
  }
}) {
  console.log(x, ...b);
};
foo({ prop: "Hello", prop2: { prop2: { nested: ["a", "b", "c"]}}});
// => Hello a b c


// In combination with other ES2015 features.

// Computed property names
const name = 'fieldName';
const computedObject = { [name]: name }; // (where object is { 'fieldName': 'fieldName' })
const { [name]: nameValue } = computedObject;
console.log(nameValue)
// => fieldName



// Rest and defaults
var ajax = function ({ url = "localhost", port: p = 80}, ...data) {
  console.log("Url:", url, "Port:", p, "Rest:", data);
};

ajax({ url: "someHost" }, "additional", "data", "hello");
// => Url: someHost Port: 80 Rest: [ 'additional', 'data', 'hello' ]

ajax({ }, "additional", "data", "hello");
// => Url: localhost Port: 80 Rest: [ 'additional', 'data', 'hello' ]


// Ooops: Doesn't work (in traceur)
var ajax = ({ url = "localhost", port: p = 80}, ...data) => {
  console.log("Url:", url, "Port:", p, "Rest:", data);
};
ajax({ }, "additional", "data", "hello");
// probably due to traceur compiler

But this does:
var ajax = ({ url: url = "localhost", port: p = 80}, ...data) => {
  console.log("Url:", url, "Port:", p, "Rest:", data);
};
ajax({ }, "additional", "data", "hello");


// Like _.pluck
var users = [
  { user: "Name1" },
  { user: "Name2" },
  { user: "Name2" },
  { user: "Name3" }
];
var names = users.map( ({ user }) => user );
console.log(names);
// => [ 'Name1', 'Name2', 'Name2', 'Name3' ]


// Advanced usage with Array Comprehension and default values
var users = [
  { user: "Name1" },
  { user: "Name2", age: 2 },
  { user: "Name2" },
  { user: "Name3", age: 4 }
];

[for ({ user, age = "DEFAULT AGE" } of users) console.log(user, age)];
// => Name1 DEFAULT AGE
// => Name2 2
// => Name2 DEFAULT AGE
// => Name3 4
```

## 2. ES6 函数的新特性

### rest 参数

ES6 引入 rest 参数（形式为...变量名），用于获取函数的多余参数，这样就不需要使用 arguments 对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。  
剩余参数用法：`function fn([arg, ] ...restArgs){}`  
**注意：rest 参数后不能有其他参数**

```js
function fn(foo, ...rest) {
  console.log(`foo: ${foo}`);
  console.log(`Rest Arguments: ${rest.join(",")}`);
}
fn(1, 2, 3, 4, 5); //foo: 1 //Rest Arguments: 2,3,4,5
```

### 函数默认参数

```js
function calc(x = 0, y = 0) {
  // ...
  console.log(x, y);
}
```

### 模版字符串

```js
$("#result").append(
  "He is <b>" +
    person.name +
    "</b>" +
    "and we wish to know his" +
    person.age +
    ".That is all"
);
```

```js
$("#result").append(
  `He is <b>${person.name}</b>and we wish to know his${person.age}.that is all`
);
```

在模板语法中调用函数：

```js
function string() {
  return "zzw likes es6!";
}
console.log(`你想说什么?
            嗯，${string()}`);
```

## 属性的简洁表示法

比如下面这样，我们想把一个名为 listeners 的数组赋值给 events 对象中的 listeners 属性，用 ES5 我们会这样做：

```js
var listeners = [];
function listen() {}
var events = {
  listeners: listeners,
  listen: listen
};
```

ES6 则允许我们简写成下面这种形式：

```js
var listeners = [];
function listen() {}
var events = { listeners, listen };
```

- 声明
  - let / const: 块级作用域、不存在变量提升、暂时性死区、不允许重复声明
  - const: 声明常量，无法修改
- 解构赋值
- `class / extend`: 类声明与继承
- `Set / Map`: 新的数据结构
- 异步解决方案:
  - Promise 的使用与实现
  - generator:
    ```
    - `yield`: 暂停代码
    - `next()`: 继续执行代码
    ```

```js
function* helloWorld() {
  yield "hello";
  yield "world";
  return "ending";
}

const generator = helloWorld();

generator.next(); // { value: 'hello', done: false }

generator.next(); // { value: 'world', done: false }

generator.next(); // { value: 'ending', done: true }

generator.next(); // { value: undefined, done: true }
```

```js
- `await / async`: 是`generator`的语法糖， babel中是基于`promise`实现。

//...js
async function getUserByAsync(){
   let user = await fetchUser();
   return user;
}

const user = await getUserByAsync()
console.log(user)
//...
```

## 3. 实现不变性

接下来演示`不变性`:

```js
> let a = [1, 2, 3]
> let b = a
> b.push(8)
> b
[1, 2, 3, 8]
> a
[1, 2, 3, 8]
```

使用`ES6`方法执行不可变操作:

- `Object.assign()`:

```js
> a = [1,2,3]
[ 1, 2, 3 ]
> b = Object.assign([],a)
[ 1, 2, 3 ]
> b.push(8)
> b
[ 1, 2, 3, 8 ] // b output
> a
[ 1, 2, 3 ] // a output
```

- `操作符(...)`

```js
> a = [1,2,3]
[ 1, 2, 3 ]
> b = [...a, 4, 5, 6]
[ 1, 2, 3, 4, 5, 6 ]
> a
[ 1, 2, 3 ]
```
