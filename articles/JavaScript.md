# JavaScript
## 目录
- [原型/构造函数/实例](#1-原型/构造函数/实例)
- [原型链](#2-原型链)
- [执行上下文(EC)](#3-执行上下文(EC))
- [变量对象](#4-变量对象)
- [作用域](#5-作用域)
- [作用域链](#6-作用域链)
- [闭包](#7-闭包)
- [script引入方式](#8-script引入方式)
- [new运算符的执行过程](#9-new运算符的执行过程)
- [instanceof原理](#10-instanceof原理)
- [代码的复用](#11-代码的复用)
- [继承](#12-继承)
- [对象的拷贝](#13-对象的拷贝)
- [类型转换](#14-类型转换)
- [类型判断](#15-类型判断)
- [模块化](#16-模块化)
- [函数执行改变this](#17-函数执行改变this)
- [AST](#18-AST)
- [babel编译原理](#19-babel编译原理)
- [函数柯里化](#20-函数柯里化)
- [数组](#21-数组)
- [String](#22-String)
## 1. 原型/构造函数/实例
- **原型**`(prototype)`: 一个简单的对象，用于实现对象的**属性继承**。可以简单的理解成对象的爹。在 Firefox 和 Chrome 中，每个`JavaScript`对象中都包含一个`__proto__ `(非标准)的属性指向它爹(该对象的原型)，可`obj.__proto__`进行访问。
- **构造函数**: 可以通过`new`来 __新建一个对象__ 的函数。
- **实例**: 通过构造函数和`new`创建出来的对象，便是实例。**实例**通过`__proto__`指向**原型**，通过constructor指向**构造函数**。

这里来举个栗子，以Object为例，我们常用的Object便是一个构造函数，因此我们可以通过它构建实例。

```js
// 实例
const instance = new Object()
```
则此时， **实例**为`instance`, **构造函数**为`Object`，我们知道，构造函数拥有一个`prototype`的属性指向原型，因此原型为:
```js
// 原型
const prototype = Object.prototype
```
这里我们可以来看出三者的关系:
```js
/* 
实例.__proto__ === 原型
原型.constructor === 构造函数
构造函数.prototype === 原型
实例.constructorr === 构造函数 
*/
instance.__proto__ === prototype 
prototype.constructor === Object
Object.prototype === prototype
instance.constructor === Object
```
![GitHub Logo](/img/原型和原型链.jpg)

## 2. 原型链
**原型链**是由**原型对象**组成，每个对象都有 `__proto__`属性，指向了创建该对象的构造函数的原型，`__proto__` 将对象连接起来组成了原型链。是一个用来**实现继承和共享属性**的有限的对象链。

- **属性查找机制**: 当查找对象的属性时，如果实例对象自身不存在该属性，则沿着原型链往上一级查找，找到时则输出，不存在时，则继续沿着原型链往上一级查找，直至最顶级的原型对象`Object.prototype`，如还是没找到，则输出`undefined`；
- **属性修改机制**: 只会修改实例对象本身的属性，如果不存在，则进行添加该属性，如果需要修改原型的属性时，则可以用: `b.prototype.x = 2`；但是这样会造成所有继承于该对象的实例的属性发生改变。

## 3. 执行上下文(EC)
执行上下文可以简单理解为一个对象:

- 它包含三个部分:
    - 变量对象(VO)
    - 作用域链(词法作用域)
    - `this`指向

- 它的类型:
    - 全局执行上下文
    - 函数执行上下文
    - `eval`执行上下文

- 代码执行过程:
    - 创建 **全局上下文** (global EC)
    - 全局执行上下文 (caller) 逐行 **自上而下** 执行。遇到函数时，**函数执行上下文** (callee) - `push`到执行栈顶层
    - 函数执行上下文被激活，成为 active EC, 开始执行函数中的代码，caller 被挂起
    - 函数执行完后，callee 被`pop`移除出执行栈，控制权交还全局上下文 (caller)，继续执行
## 4. 变量对象
变量对象，是执行上下文中的一部分，可以抽象为一种 **数据作用域**，其实也可以理解为就是一个简单的对象，它存储着该执行上下文中的所有 **变量和函数声明(不包含函数表达式)**。

    活动对象 (AO): 当变量对象所处的上下文为 active EC 时，称为活动对象。
## 5. 作用域
执行上下文中还包含作用域链。理解作用域之前，先介绍下作用域。作用域其实可理解为该上下文中声明的 **变量和声明的作用范围**。可分为 **块级作用域** 和 **函数作用域**

特性:

- **声明提前**: 一个声明在函数体内都是可见的, 函数优先于变量
- 非匿名自执行函数，函数变量为 只读 状态，无法修改
```js
const foo = 1
(function foo() {
    foo = 10  // 由于foo在函数中只为可读，因此赋值无效
    console.log(foo)
}()) 

// 结果打印：  ƒ foo() { foo = 10 ; console.log(foo) }
```
## 6. 作用域链
我们知道，我们可以在执行上下文中访问到父级甚至全局的变量，这便是作用域链的功劳。作用域链可以理解为一组对象列表，包含 **父级和自身的变量对象**，因此我们便能通过作用域链访问到父级里声明的变量或者函数。

- 由两部分组成:
    - `[[scope]]`属性: 指向父级变量对象和作用域链，也就是包含了父级的`[[scope]]`和`AO`
    - **AO**: 自身活动对象

如此 `[[scopr]]`包含`[[scope]]`，便自上而下形成一条 **链式作用域**。
## 7. 闭包
闭包属于一种特殊的作用域，称为 **静态作用域**。它的定义可以理解为: **父函数被销毁** 的情况下，返回出的子函数的`[[scope]]`中仍然保留着父级的单变量对象和作用域链，因此可以继续访问到父级的变量对象，这样的函数称为闭包。

- 闭包会产生一个很经典的问题:
    - 多个子函数的`[[scope]]`都是同时指向父级，是完全共享的。因此当父级的变量对象被修改时，所有子函数都受到影响。

- 解决:
    - 变量可以通过 **函数参数的形式** 传入，避免使用默认的`[[scope]]`向上查找
    - 使用`setTimeout`包裹，通过第三个参数传入
    - 使用 **块级作用域**，让变量成为自己上下文的属性，避免共享
## 8. script 引入方式：
- html 静态`<script>`引入
- js 动态插入`<script>`
- `<script defer>`: 异步加载，元素解析完成后执行
- `<script async>`: 异步加载，与元素渲染并行执行
## 9. new运算符的执行过程
- 新生成一个对象
- 链接到原型: `obj.__proto__ = Con.prototype`
- 绑定this: `apply`
- 返回新对象
## 10. instanceof原理
能在实例的 **原型对象链** 中找到该构造函数的`prototype`属性所指向的 **原型对象**，就返回`true`。即:
```js
// __proto__: 代表原型对象链
instance.[__proto__...] === instance.constructor.prototype

// return true
```
## 11. 代码的复用
当你发现任何代码开始写第二遍时，就要开始考虑如何复用。一般有以下的方式:
- 函数封装
- 继承
- 复制`extend`
- 混入`mixin`
- 借用`apply/call`
## 12. 继承
在 JS 中，继承通常指的便是 **原型链继承**，也就是通过指定原型，并可以通过原型链继承原型上的属性或者方法。
- 最优化: 圣杯模式 
```js
var inherit = (function(c,p){
    var F = function(){};
    return function(c,p){
        F.prototype = p.prototype;
        c.prototype = new F();
        c.uber = p.prototype;
        c.prototype.constructor = c;
    }
})();
```
- 使用 ES6 的语法糖 `class` / `extends` 
## 13. 对象的拷贝
- 浅拷贝: 以赋值的形式拷贝引用对象，仍指向同一个地址，修改时原对象也会受到影响
    - `Object.assign()`   
    用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。
    - 展开运算符(...)
- 深拷贝: 完全拷贝一个新对象，修改时原对象不再受到任何影响
    - `JSON.parse(JSON.stringify(obj))`: 性能最快
        - 具有循环引用的对象时，报错
        - 当值为函数或`undefined`时，无法拷贝
    - 递归进行逐一赋值
## 14. 类型转换
JS 中在使用运算符号或者对比符时，会自带隐式转换，规则如下:
- -、*、/、% ：一律转换成数值后计算
- +：
    - 数字 + 字符串 = 字符串， 运算顺序是从左到右
    - 数字 + 对象， 优先调用对象的valueOf -> toString
    - 数字 + `boolean/null` = 数字
    - 数字 + `undefined` == `NaN`
- `[1].toString() `===` '1'`
- `{}.toString()` === `'[object object]'`
- `NaN` !== `NaN` 、`+undefined` === `NaN`
## 15. 类型判断
判断 Target 的类型，单单用 `typeof` 并无法完全满足，这其实并不是 bug，本质原因是 JS 的万物皆对象的理论。因此要真正完美判断时，我们需要区分对待:
- 基本类型(`null`): 使用 `String(null)`
- 基本类型(`string` / `number` / `boolean` / `undefined`) + `function`: 直接使用 `typeof`即可
- 判断已知对象类型的方法 `instanceof`
- 其余引用类型(`Array` / `Date` / `RegExp` `Error`): 调用`toString`后根据`[object XXX]`进行判断  

很稳的判断封装:
```js
let class2type = {}
'Array Date RegExp Object Error'.split(' ').forEach(
    e => class2type[ '[object ' + e + ']' ] = e.toLowerCase()
) 

function type(obj) {
    if (obj == null) return String(obj)
    return typeof obj === 'object' 
        ? class2type[ Object.prototype.toString.call(obj) ] || 'object' : typeof obj
}
```
**原理**：
```js
Object.toString()//"function Object() { [native code] }"
Object.prototype.toString()//"[object Object]"
```
`Object`对象和它的原型链上各自有一个`toString()`方法，第一个返回的是一个函数，第二个返回的是值类型。  
既然知道了不同，现在再来分析下`Object.prototype.toString.call(Array)//"[object Function]"`。  
`Array`对象本身返回一个构造函数，  
`Array//ƒ Array() { [native code] }`，  
而`Object.prototype.toString()`返回的是`//"[object Type]"`的形式，  
通过`call`将`Array`的`this上`下文切换到`Object`，从而调用了`Object.prototype.toString()`，  
因此返回`[object Function]`。

那么不可以直接`Array.prototype.toString.call([1,3,4])`吗？
不行！
因为`Array`，`Function`，`Date`虽然是基于`Object`进行创建的，但是他们继承的是`Object.toString()`，而不是`Object.prototype.toString()`。
```js
Array.toString()       // "function Array() { [native code] }"
Array.prototype.toString()          // ""
```

## 16. 模块化
模块化开发在现代开发中已是必不可少的一部分，它大大提高了项目的可维护、可拓展和可协作性。通常，__我们 在浏览器中使用 ES6 的模块化支持，在 Node 中使用 commonjs 的模块化支持__。
- 分类:
    - es6: `import` / `exports`
    - commonjs: `require / module.exports / exports`
    - amd: `require / defined`
- `require`与`import`的区别
    - `require`支持 **动态导入**，`import`不支持，正在提案 (babel 下可支持)
    - `require`是 **同步** 导入，`import`属于 **异步** 导入
    - `require`是 **值拷贝**，导出值变化不会影响导入值；`import`指向 **内存地址**，导入值会随导出值而变化
## 17. 函数执行改变this
由于 JS 的设计原理: 在函数中，可以引用运行环境中的变量。因此就需要一个机制来让我们可以在函数体内部获取当前的运行环境，这便是`this`。

因此要明白 `this` 指向，其实就是要搞清楚 函数的运行环境，说人话就是，谁调用了函数。例如:
- `obj.fn()`，便是 obj 调用了函数，既函数中的 `this` === `obj`
- `fn()`，这里可以看成 `window.fn()`，因此 `this` === `window`  

但这种机制并不完全能满足我们的业务需求，因此提供了三种方式可以手动修改 `this` 的指向:
- `call: fn.call(target, 1, 2)`
- `apply: fn.apply(target, [1, 2])`
- `bind: fn.bind(target)(1,2)`
## 18. AST
**抽象语法树 (Abstract Syntax Tree)**，是将代码逐字母解析成 **树状对象** 的形式。这是语言之间的转换、代码语法检查，代码风格检查，代码格式化，代码高亮，代码错误提示，代码自动补全等等的基础。例如:
```js
function square(n){
    return n * n
}
```
通过解析转化成的AST如下图:
![imgTitle](/img/AST.png)
## 19. babel编译原理
- babylon 将 ES6/ES7 代码解析成 AST
- babel-traverse 对 AST 进行遍历转译，得到新的 AST
- 新 AST 通过 babel-generator 转换成 ES5
## 20. 函数柯里化
在一个函数中，首先填充几个参数，然后再返回一个新的函数的技术，称为函数的柯里化。通常可用于在不侵入函数的前提下，为函数 **预置通用参数**，供多次重复调用。
```javascript {.line-numbers}
const add = function add(x) {
    return function (y) {
        return x + y
    }
}

const add1 = add(1)

add1(2) === 3
add1(20) === 21
```
## 21. 数组
- `map`: 遍历数组，返回回调返回值组成的新数组  
    `array.map(function(currentValue, index, arr), thisValue)`
- `forEach`: 无法`break`，可以用`try/catch`中`throw new Error`来停止  
    `array.forEach(function(currentValue, index, arr), thisValue)`
- `filter`: 过滤、筛选,会返回过滤后的新数组。用法跟 map 极为相似：
    `array.filter(function(currentValue, index, arr), thisValue)`
    `filter`的`callback`函数，需要返回布尔值`true`或`false`。返回值只要**弱等于**`Boolean`就行，看下面这个例子：
```js
const data = [0, 1, 2, 3];
const arrayFilter = data.filter(function(item) {
    return item;
});
console.log(arrayFilter); // [1, 2, 3]
```
- `some`: 有一项返回`true`，则整体为`true`
- `every`: 有一项返回`false`，则整体为`false`
- `join`: 通过指定连接符生成字符串
```js
var arr = ["George","John","Thomas"]
arr.join("-")       // George-John-Thomas
```
- `push / pop`: 末尾推入和弹出，改变原数组， 返回推入/弹出项
- `unshift / shift`: 头部推入和弹出，改变原数组，返回操作项
- `sort(fn) / reverse`: 排序与反转，改变原数组
- `concat`: 连接数组，不影响原数组， 浅拷贝
- `slice(start, end)`: 返回截断后的新数组，不改变原数组
```js
// start 必需。规定从何处开始选取。如果是负数，那么它规定从数组尾部开始算起的位置。也就是说，-1 指最后一个元素，-2 指倒数第二个元素，以此类推。
// end 可选。规定从何处结束选取。该参数是数组片断结束处的数组下标。如果没有指定该参数，那么切分的数组包含从 start 到数组结束的所有元素。如果这个参数是负数，那么它规定的是从数组尾部开始算起的元素。

var arr = ["George", "John", "Thomas", "James", "Adrew", "Martin"]
arr.slice(2,0)  // []
arr.slice(2,1)  // []
arr.slice(2,2)  // []
arr.slice(2,3)  // ["Thomas"]
arr.slice(2,4)  // ["Thomas", "James"]
arr.slice(2,5)  // ["Thomas", "James", "Adrew"]
arr.slice(2,6)  // ["Thomas", "James", "Adrew", "Martin"]
arr
// ["George", "John", "Thomas", "James", "Adrew", "Martin"]
```
- `splice(start, number, value...)`: 返回删除元素组成的数组，value 为插入项，改变原数组
```js
arrayObject.splice(index,howmany,item1,.....,itemX)
// index 	必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
// howmany 	必需。要删除的项目数量。如果设置为 0，则不会删除项目。
// item1, ..., itemX 	可选。向数组添加的新项目。
var arr = ["George","John","Thomas", "James"]
arr.splice(2,0,"William")
// "George","John","Thomas","William", "James"
arr.splice(2,1,"William")
// ["George", "John", "William", "James"]
arr.splice(2,3,"William")
// ["Thomas", "James"]
```
- `indexOf / lastIndexOf(value, fromIndex)`: 查找数组项，返回对应的下标
- `reduce / reduceRight(fn(prev, cur)， defaultPrev)`:   
两两执行，prev 为上次化简函数的return值，cur 为当前值(从第二项开始)
- `fill()`方法用于将一个固定值替换数组的元素
```js
array.fill(value, start, end)
// alue 	必需。填充的值。
// start 	可选。开始填充位置。
// end 	可选。停止填充位置 (默认为 array.length)

var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.fill("Runoob");       // Runoob,Runoob,Runoob,Runoob 
```
- `find()` 用于找出数组中符合条件的第一个元素，并不是用于遍历数组
- 数组乱序：
```js
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
arr.sort(function () {
    return Math.random() - 0.5;
});
```
- 数组拆解: `flat: [1,[2,3]] --> [1, 2, 3]`
```js
arr.prototype.flat = function() {
    this.toString().split(',').map(item => +item )
}
```
### 数组遍历和对象遍历
针对js各种遍历作一个总结分析，从类型用处：分数组遍历和对象遍历；还有性能，优缺点等。
JS数组遍历：

1. 普通`for`、`while`、`do-while`、`for-in`循环，经常用的数组遍历
```js
var arr = [1,2,0,3,9];
//for
for ( var i = 0; i < arr.length; i ++){
    console.log(arr[i]);
}
//while
var k = 0;
while(k < num.length){
    console.log(arr[k]);
    k ++;
}
//do-while
var k = 0;
do{
    console.log(arr[k]);
    k ++;
}while(k < num.length)
//for-in
var i;
for(i in arr){
    console.log(arr[i]);
}
```
2. 优化版`for`循环:使用变量，将长度缓存起来，避免重复获取长度，数组很大时优化效果明显
```js
for(var j = 0,len = arr.length; j < len; j++){
    console.log(arr[j]);
}
```
3. `forEach`，`ES5`推出的,数组自带的循环，主要功能是遍历数组，实际性能比for还弱
```js
arr.forEach(function(value,i){
　　console.log('forEach遍历:'+i+'--'+value);

})
```
`forEach`这种方法也有一个小缺陷：你不能使用`break`语句中断循环，也不能使用return语句返回到外层函数。

4. `map`遍历，`map`即是 “映射”的意思 用法与 `forEach` 相似
```js
arr.map(function(value,index){
    console.log('map遍历:'+index+'--'+value);
});
```
`map`遍历支持使用`return`语句，支持`return`返回值
```js
var temp=arr.map(function(val,index){
    console.log(val);  
    return val*val           
})
console.log(temp);  
```
`forEach`、`map`都是`ECMA5`新增数组的方法，所以ie9以下的浏览器还不支持

5. `for-of`遍历 是`ES6`新增功能
```js
for( let i of arr){
    console.log(i);
}
```

`for-of`这个方法避开了`for-in`循环的所有缺陷,与forEach()不同的是，它可以正确响应`break`、`continue`和`return`语句;

`for-of`循环不仅支持数组，还支持大多数类数组对象，例如`DOM NodeList`对象。
`for-of`循环也支持字符串遍历.

### JS对象遍历：
1. for-in遍历  

for-in是为遍历对象而设计的，不适用于遍历数组。
遍历数组的缺点：数组的下标index值是数字，for-in遍历的index值"0","1","2"等是字符串
```js
for (var index in arr){
    console.log(arr[index]);
    console.log(index);
}
```
2. 使用Object.keys()遍历  

返回一个数组,包括对象自身的(不含继承的)所有可枚举属性(不含Symbol属性).
```js
var obj = {'0':'a','1':'b','2':'c'};
Object.keys(obj).forEach(function(key){
    console.log(key,obj[key]);
});
```
3. 使用Object.getOwnPropertyNames(obj)遍历

返回一个数组,包含对象自身的所有属性(不含Symbol属性,但是包括不可枚举属性).
```js
var obj = {'0':'a','1':'b','2':'c'};
Object.getOwnPropertyNames(obj).forEach(function(key){
    console.log(key,obj[key]);
});
```
4. 使用Reflect.ownKeys(obj)遍历

返回一个数组,包含对象自身的所有属性,不管属性名是Symbol或字符串,也不管是否可枚举.  
```js
var obj = {'0':'a','1':'b','2':'c'};
Reflect.ownKeys(obj).forEach(function(key){
    console.log(key,obj[key]);
});
```

如何让页面上的所有图片全部隐藏？
```js
let imglist = document.getElementsByTagName('img')
imglist instanceof Object === true
Object.prototype.toString.call(imglist) // "[object HTMLCollection]"
// imglist.map((value,index)=>{
//     imglist[index].style.display = 'none'
// })
// for(index in imglist){
//     imglist[index].style.display = 'none'
// }
Object.getOwnPropertyNames(imglist).forEach((key)=>{
    imglist[key].style.display = 'none'
})
```
## 22. String
- `slice()`: 提取字符串的片断，并在新的字符串中返回被提取的部分。
```js
stringObject.slice(start,end)
// start 	要抽取的片断的起始下标。如果是负数，则该参数规定的是从字符串的尾部开始算起的位置。也就是说，-1 指字符串的最后一个字符，-2 指倒数第二个字符，以此类推。
// end 	紧接着要抽取的片段的结尾的下标。若未指定此参数，则要提取的子串包括 start 到原字符串结尾的字符串。如果该参数是负数，那么它规定的是从字符串的尾部开始算起的位置。

var str="Hello happy world!"
str.slice(6)    // happy world!
str.slice(6,11)     // happy
```
- `split()`: 把字符串分割为字符串数组。
```js
// 手写一个cookie解析的方法
const parseCookie = function(str){
    let ckArr = document.cookie.split(';')
    for(let i=0;i<ckArr.length;i++){
        if(ckArr[i].indexOf(str)!==-1){
            return ckArr[i].split("=")[1]
        }
    }
}

```
- `substr()`: 从起始索引号提取字符串中指定数目的字符。
- `substring()`: 提取字符串中两个指定的索引号之间的字符。