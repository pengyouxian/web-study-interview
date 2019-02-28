# ES6/ES7
由于 Babel 的强大和普及，现在 ES6/ES7 基本上已经是现代化开发的必备了。通过新的语法糖，能让代码整体更为简洁和易读。
- 声明
    - let / const: 块级作用域、不存在变量提升、暂时性死区、不允许重复声明
    - const: 声明常量，无法修改
- 解构赋值
- `class / extend`: 类声明与继承
- `Set / Map`: 新的数据结构
- 异步解决方案:
    - Promise的使用与实现
    - generator:
        ```
        - `yield`: 暂停代码 
        - `next()`: 继续执行代码
        ```
```
function* helloWorld() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

const generator = helloWorld();

generator.next()  // { value: 'hello', done: false }

generator.next()  // { value: 'world', done: false }

generator.next()  // { value: 'ending', done: true }

generator.next()  // { value: undefined, done: true }
```
```
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


