# nodejs
---
# node开发技能图解
![](/img/node开发技能图解.jpg)

[🔗.node.js面试题大全－侧重后端应用与对Node核心的理解](https://www.cnblogs.com/meteorcn/p/node_mianshiti_interview_question.html)

## 1.Nodejs的优缺点
**优点**：
- 事件驱动，通过闭包很容易实现客户端的生命活期。
- 不用担心多线程，锁，并行计算的问题
- V8引擎速度非常快
- 对于游戏来说，写一遍游戏逻辑代码，前端后端通用
- 简单强大，轻量可扩展．简单体现在node使用的是javascript,json来进行编码，人人都会；
- 强大体现在非阻塞IO,可以适应分块传输数据，较慢的网络环境，尤其擅长高并发访问；
- 轻量体现在node本身既是代码，又是服务器，前后端使用统一语言;
- 可扩展体现在可以轻松应对多实例，多服务器架构，同时有海量的第三方应用组件。

**缺点**：
- nodejs更新很快，可能会出现版本兼容
- nodejs还不算成熟，还没有大制作
- nodejs不像其他的服务器，对于不同的链接，不支持进程和线程操作

## 2.什么是错误优先的回调函数？
- 错误优先(Error-first)的回调函数（Error-First Callback）用于同时返回错误和数据。第一个参数返回错误，并且验证它是否出错；其他参数返回数据。
```js
fs.readFile(filePath, function(err, data){
    if (err){
        // 处理错误
        return console.log(err);
    }
    console.log(data);
});
```
Q: node API中的 **回调函数** 的参数都遵循什么惯例？
A: `err`一般为第一个参数，这被称为 **错误优先的回调函数**
## 3.如何避免回调地狱？
以下方式避免回调地狱:
- 模块化：将回调函数转换为独立的函数
- 使用流程控制库，例如[aync]
- 使用Promise
- 使用aync/await
## 4.什么是`Promise`?
- `Promise`可以帮助我们更好地处理异步操作。
下面的实例中，100ms后会打印result字符串。catch用于错误处理。
多个Promise可以链接起来。
```js
new Promise((resolve, reject) =>{
        setTimeout(() =>{
            resolve('result');
        }, 100)
    })
    .then(console.log)
    .catch(console.error);
```
## 5.使用NPM有哪些好处？
- 通过NPM，你可以安装和管理项目的依赖，并且能够指明依赖项的具体版本号。
- 对于Node应用开发而言，可以通过package.json文件来管理项目信息，配置脚本，以及指明依赖的具体版本
## 6.node有哪些核心模块?
- `EventEmitter`, `Stream`, `FS`, `Net`和 **全局对象**(`global`)