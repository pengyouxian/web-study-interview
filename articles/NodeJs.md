# nodejs
---

从Node.js进入我们的视野时，我们所知道的它就由这些关键字组成 事件驱动、非阻塞I/O、高效、轻量，它在官网中也是这么描述自己的。

Node.js® is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient.

---

# node开发技能图解
![](/img/node开发技能图解.jpg)

[🔗.node.js面试题大全－侧重后端应用与对Node核心的理解](https://www.cnblogs.com/meteorcn/p/node_mianshiti_interview_question.html)

1. 什么是回调？  
回调是异步编程时的基础，将后续逻辑封装成起始函数的参数，逐层嵌套 
2. 什么是同步/异步？  
同步是指：发送方发出数据后，等接收方发回响应以后才发下一个数据包的通讯方式。   
异步是指：发送方发出数据后，不等接收方发回响应，接着发送下个数据包的通讯方式。   
3. 什么是I/O？  
磁盘的写入（in）磁盘的读取（out） 
4. 什么的单线程/多线程？  
一次只能执行一个程序叫做单线程,一次能执行多个程序叫多线程 
5. 什么是阻塞/非阻塞？  
阻塞：前一个程序未执行完就得一直等待  
非阻塞：前一个程序未执行完时可以挂起，继续执行其他程序，等到使用时再执行 
6. 什么是事件？  
一个触发动作（例如点击按钮） 
7. 什么是事件驱动？  
一个触发动作引起的操作（例如点击按钮后弹出一个对话框） 
8. 什么是基于事件驱动的回调？  
为了某个事件注册了回调函数，但是这个回调函数不是马上执行，只有当事件发生的时候，才会调用回调函数，这种函数执行的方式叫做**事件驱动**~  
这种注册回调就是基于事件驱动的回调，如果这些回调和异步I/O(数据写入、读取)操作有关，可以看作是基于回调的异步I/O，只不过这种回调在nodejs中是有事件来驱动的 
9. 什么是事件循环？  
//事件循环Eventloop,倘若有大量的异步操作，一些I/O的耗时操作，甚至是一些定时器控制的延时操作，它们完成的时候都要调用相应的回调函数，从而来完成一些密集的任务，而又不会阻塞整个程序执行的流程，此时需要一种机制来管理，这种机制叫做事件循环. 总而言之就是：管理大量异步操作的机制叫做事件循环 
Event Loop: 回调函数队列。异步执行的函数会被压入这个队列; 队列被循环查询

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
