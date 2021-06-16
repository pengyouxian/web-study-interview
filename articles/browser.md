# 浏览器相关
前面两节是关于性能优化的
## 方法：
- [性能优化](#1-性能优化)
    - [防抖与节流](#I-防抖与节流)
    - [重绘与回流](#II-重绘与回流)
    - [减少dom操作](#III-减少dom操作)
- [跨标签页通讯](#2-跨标签页通讯)
- [浏览器架构](#3-浏览器架构)
- [浏览器下事件循环(Event Loop)](#4-浏览器下事件循环(EventLoop))
- [从输入url到展示的过程](#5-从输入url到展示的过程)
- [存储](#6-存储)
- [Web Worker](#7-WebWorker)
- [V8垃圾回收机制](#8-V8垃圾回收机制)
- [内存泄露](#9-内存泄露)
- [GET和POST的区别](#10-GET和POST的区别)
- [跨域](#11-跨域)
## 1.性能优化
**性能优化原则**：
- 多使用内存、缓存或者其他方法
- 减少CPU计算、减少网络
 
从哪里入手：
- 加载页面和静态资源
- 页面渲染

具体的：
- 加载资源优化：
    - 静态资源的压缩合并
    - 静态资源缓存
    - 使用CDN让资源加载更快
    - 使用SSR后端渲染，数据直接输出到HTML中
 
- 渲染优化：
    - CSS放前面，JS放后面
    - 懒加载（图片懒加载、下拉加载更多）
    - 减少DOM查询，对DOM查询做缓存
    - 减少DOM操作，多个操作尽量合并在一起执行
    - 事件节流
    - 尽早执行操作（如DOMContentLoaded）
### I. 防抖与节流
防抖与节流函数是一种最常用的 高频触发优化方式，能对性能有较大的帮助。
- **防抖**(debounce): 将多次高频操作优化为只在最后一次执行，通常使用的场景是：用户输入，只需再输入完成后做一次输入校验即可。
```javascript {.line-numbers}
function debounce(fn, wait, immediate) {
    let timer = null

    return function() {
        let args = arguments
        let context = this

        if (immediate && !timer) {
            fn.apply(context, args)
        }

        if (timer) clearTimeout(timer)
        timer = setTimeout(() => {
            fn.apply(context, args)
        }, wait)
    }
}
```
- **节流**(throttle): 每隔一段时间后执行一次，也就是降低频率，将高频操作优化成低频操作，通常使用场景: 滚动条事件 或者 resize 事件，通常每隔 100~500 ms执行一次即可。
```javascript {.line-numbers}
function throttle(fn, wait, immediate) {
    let timer = null
    let callNow = true
    
    return function() {
        let context = this,
            args = arguments

        if (callNow) {
            fn.apply(context, args)
            callNow = false
        }

        if (!timer) {
            timer = setTimeout(() => {
                fn.apply(context, args)
                timer = null
            }, wait)
        }
    }
}
```
### II. 重绘与回流
当元素的样式发生变化时，浏览器需要触发更新，重新绘制元素。这个过程中，有两种类型的操作，即重绘与回流。
- **重绘(repaint)**: 当元素样式的改变不影响布局时，浏览器将使用重绘对元素进行更新，此时由于只需要UI层面的重新像素绘制，因此 损耗较少
- **回流(reflow)**: 当元素的尺寸、结构或触发某些属性时，浏览器会重新渲染页面，称为回流。此时，浏览器需要重新经过计算，计算后还需要重新页面布局，因此是较重的操作。会触发回流的操作:
    - 页面初次渲染
    - 浏览器窗口大小改变
    - 元素尺寸、位置、内容发生改变

    - 元素字体大小变化
        - 添加或者删除可见的 dom 元素
    - 激活 CSS 伪类（例如：:hover）
    - 查询某些属性或调用某些方法
        ```
        - clientWidth、clientHeight、clientTop、clientLeft
        - offsetWidth、offsetHeight、offsetTop、offsetLeft
        - scrollWidth、scrollHeight、scrollTop、scrollLeft
        - getComputedStyle()
        - getBoundingClientRect()
        - scrollTo()
        ```
**回流必定触发重绘，重绘不一定触发回流。重绘的开销较小，回流的代价较高**
最佳实践:
- css
    - 避免使用`table`布局
    - 将动画效果应用到`position`属性为`absolute`或`fixed`的元素上
- javascript
    - 避免频繁操作样式，可汇总后统一 **一次修改**
    - 尽量使用`class`进行样式修改
    - 减少`dom`的增删次数，可使用 **字符串** 或者 `documentFragment` 一次性插入
    - 极限优化时，修改样式可将其`display: none`后修改
    - 避免多次触发上面提到的那些会触发回流的方法，可以的话尽量用 **变量存住**
### III. 减少dom操作
- 先把dom做好，然后再插入。
- dom操作的效率提高了，这确实是以前没有考虑到的
示例代码：
```javascript {.line-numbers}
// 需求：插入10个li标签
var listNode = document.getElementById('list');

var frag = document.createDocumentFragment();
var x, li;
for(x = 0;x < 10; x ++){
    li = document.createElement('li');
    li.innerHtml = "List item" + x;
    frag.appendChild(li);
}
listNode.appendChild(frag);
```
- 缓存dom查询的结果,不然每一次循环都要查一次
```javascript {.line-numbers}
// 未缓存dom查询
for(var i=0;i = document.getElementByTagName('p').length;i++){
    // todo
}

//缓存dom查询的结果
var plist = document.getElementByTagName('p')
for(var i=0;i = plist.length;i++){
    // todo
}
```
- 懒加载
```html
<img id='img1' src='greview.png' data-realsrc='abc.png'>
<script type='text/javascript'>
    var img = document.getElementById('img1')
    img1.src = img1.getArribute('data-realsrc')
</script>
```
- 尽早操作
```js
window.addEventListener('load',function(){
    //  页面的全部资源全部加载才会执行，包括图片等资源
})
window.addEventListener('DOMContentloaded',function(){
    //  dom渲染完即可执行，此时图片等资源还未加载
})
```


## 2. 跨标签页通讯
不同标签页间的通讯，本质原理就是去运用一些可以 **共享的中间介质**，因此比较常用的有以下方法:
- 通过父页面`window.open()`和子页面`postMessage`
    - 异步下，通过 `window.open('about: blank')` 和 `tab.location.href = '*' `
- 设置同域下共享的`localStorage`与监听`window.onstorage`
    - 重复写入相同的值无法触发
    - 会受到浏览器隐身模式等的限制
- 设置共享`cookie`与不断轮询脏检查`(setInterval)`
- 借助服务端或者中间层实现
## 3. 浏览器架构
- 用户界面
- 主进程
- 内核
    - 渲染引擎
    - JS 引擎
        - 执行栈
    - 事件触发线程
        - 消息队列
            - 微任务
            - 宏任务
    - 网络异步线程
    - 定时器线程
## 4. 浏览器下事件循环(EventLoop)
事件循环是指: 执行一个宏任务，然后执行清空微任务列表，循环再执行宏任务，再清微任务列表
- 微任务 `microtask(jobs)`: `promise / ajax / Object.observe`
- 宏任务 `macrotask(task)`: `setTimout / script / IO / UI Rendering`
## 5. 从输入url到展示的过程
- DNS 解析
- TCP 三次握手
- 发送请求，分析 url，设置请求报文(头，主体)
- 服务器返回请求的文件 (html)
- 浏览器渲染 :
    - 根据HTML结构生成DOM Tree;
    - 根据CSS 生成CSSOM ;
    - 根据DOM 和CSSOM 整合形成Render Tree;
    - 根据Render Tree 开始渲染和展示
    - 遇到`<script>`时，会执行并阻塞渲染
## 6. 存储
需要对业务中的一些数据进行存储，通常可以分为 **短暂性存储** 和 **持久性储存**。  
- 短暂性的时候，我们只需要将数据存在内存中，只在运行时可用
- 持久性存储，可以分为 浏览器端 与 服务器端
    - 浏览器:
        - `cookie`: 通常用于存储用户身份，登录状态等
            - `http` 中自动携带，体积上限为 4K，可自行设置过期时间
        - `localStorage/sessionStorage`: 长久储存/窗口关闭删除，体积限制为 4~5M
        - `indexDB`
    - 服务器:
        - 分布式缓存 redis
        - 数据库

<table>
   <tr>
      <td>特性</td>
      <td>Cookie</td>
      <td>localStorage</td>
      <td>sessionStorage</td>
   </tr>
   <tr>
      <td>数据的生命期</td>
      <td>一般由服务器生成，可设置失效时间。如果在浏览器端生成Cookie，默认是关闭浏览器后失效</td>
      <td>除非被清除，否则永久保存</td>
      <td>仅在当前会话下有效，关闭页面或浏览器后被清除</td>
   </tr>
   <tr>
      <td>存放数据大小</td>
      <td>4K左右</td>
      <td colspan="2">一般为5MB</td>
   </tr>
   <tr>
      <td>与服务器端通信</td>
      <td>每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题</td>
      <td colspan="2">仅在客户端（即浏览器）中保存，不参与和服务器的通信</td>
   </tr>
   <tr>
      <td>易用性</td>
      <td>需要程序员自己封装，源生的Cookie接口不友好</td>
      <td colspan="2">源生接口可以接受，可以直接通过setItem(key,value)/getItem(key)调用,亦可再次封装来对Object和Array有更好的支持</td>
   </tr>
   <tr>
      <td>是否会携带到ajax中</td>
      <td>每次都会在请求中携带</td>
      <td colspan="2">存储在本地</td>
   </tr>
</table>

[🔗表格制作的参考链接](http://pressbin.com/tools/excel_to_html_table/index.html)

## 7. WebWorker
现代浏览器为`JavaScript`创造的 **多线程环境**。可以新建并将部分任务分配到`worker`线程并行运行，两个线程可 **独立运行，互不干扰**，可通过自带的 **消息机制** 相互通信。
基本用法:
```js
// 创建 worker
const worker = new Worker('work.js');

// 向主进程推送消息
worker.postMessage('Hello World');

// 监听主进程来的消息
worker.onmessage = function (event) {
  console.log('Received message ' + event.data);
}
```
限制:
- 同源限制
- 无法使用 `document` / `window` / `alert` / `confirm`
- 无法加载本地资源
## 8. V8垃圾回收机制
**垃圾回收**: 将内存中不再使用的数据进行清理，释放出内存空间。V8 将内存分成 **新生代空间** 和 *老生代空间*。
- **新生代空间**: 用于存活较短的对象
    - 又分成两个空间: from 空间 与 to 空间
    - Scavenge GC算法: 当 from 空间被占满时，启动 GC 算法
        - 存活的对象从 from space 转移到 to space
        - 清空 from space
        - from space 与 to space 互换
        - 完成一次新生代GC
- **老生代空间**: 用于存活时间较长的对象
    - 从 新生代空间 转移到 老生代空间 的条件
        - 经历过一次以上 Scavenge GC 的对象
        - 当 to space 体积超过25%
    - 标记清除算法: 标记存活的对象，未被标记的则被释放
        - 增量标记: 小模块标记，在代码执行间隙执，GC 会影响性能
        - 并发标记(最新技术): 不阻塞 js 执行
    - 压缩算法: 将内存中清除后导致的碎片化对象往内存堆的一端移动，解决 内存的碎片化
## 9. 内存泄露
- 意外的**全局变量**: 无法被回收
- **定时器**: 未被正确关闭，导致所引用的外部变量无法被释放
- **事件监听**: 没有正确销毁 (低版本浏览器可能出现)
- **闭包**: 会导致父级中的变量无法被释放
- **dom 引用**: dom 元素被删除时，内存中的引用未被正确清空
可用 chrome 中的 timeline 进行内存标记，可视化查看内存的变化情况，找出异常点。

## 10. GET和POST的区别
### 1. 标准答案
分类|GET|POST
:----|:----|:----
后退按钮/刷新|无害|数据会被重新提交（浏览器应该告知用户数据会被重新提交）。
书签|可收藏为书签|不可收藏为书签
缓存|能被缓存|不能缓存(除非手动设置)
编码类型|application/x-www-form-urlencode。url编码|application/x-www-form-urlencoded 或 multipart/form-data。为二进制数据使用多重编码。
历史|参数保留在浏览器历史中。|参数不会保存在浏览器历史中。
对数据长度的限制|是的。当发送数据时，GET 方法向 URL 添加数据；URL 的长度是受限制的（URL 的最大长度是 2048 个字符）。|无限制。
对数据类型的限制|只允许 ASCII 字符。|没有限制。也允许二进制数据。
安全性|与 POST 相比，GET 的安全性较差，因为所发送的数据是 URL 的一部分。在发送密码或其他敏感信息时绝不要使用 GET ！|POST 比 GET 更安全，因为参数不会被保存在浏览器历史或 web 服务器日志中。
可见性|数据在 URL 中对所有人都是可见的。|数据不会显示在 URL 中。
  
注意，并不是说标准答案有误，上述区别在**大部分浏览器**上是存在的，因为这些浏览器实现了 HTTP 标准。

所以从标准上来看，GET 和 POST 的区别如下：
- `GET` 用于获取信息，是无副作用的，是幂等的，且可缓存
- `POST` 用于修改服务器上的数据，有副作用，非幂等，不可缓存
但是，既然本文从报文角度来说，那就先不讨论 RFC 上的区别，单纯从数据角度谈谈。

[GET和POST两种基本请求方法的区别](https://www.cnblogs.com/logsharing/p/8448446.html)

### 2. GET 和 POST 报文上的区别
先下结论，**GET 和 POST 方法没有实质区别**，只是报文格式不同。
GET 和 POST 只是 HTTP 协议中两种请求方式，而 HTTP 协议是基于 TCP/IP 的应用层协议，无论 GET 还是 POST，用的都是同一个传输层协议，所以在传输上，没有区别。
报文格式上，不带参数时，最大区别就是第一行方法名不同
POST方法请求报文第一行是这样的 `POST /uri HTTP/1.1 \r\n`
GET方法请求报文第一行是这样的 `GET /uri HTTP/1.1 \r\n`
是的，不带参数时他们的区别就仅仅是报文的前几个字符不同而已
带参数时报文的区别呢？ 在约定中，GET 方法的参数应该放在 url 中，POST 方法参数应该放在 body 中
举个例子，如果参数是 name=qiming.c, age=22。
GET 方法简约版报文是这样的:
```js
GET /index.php?name=qiming.c&age=22 HTTP/1.1
Host: localhost
```
POST 方法简约版报文是这样的:
```js
POST /index.php HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

name=qiming.c&age=22
```
现在我们知道了两种方法本质上是 TCP 连接，没有差别，也就是说，如果我不按规范来也是可以的。我们可以在 URL 上写参数，然后方法使用 POST；也可以在 Body 写参数，然后方法使用 GET。当然，这需要服务端支持。
### 3. 常见问题
#### 1. GET 方法参数写法是固定的吗？
在约定中，我们的参数是写在 `?` 后面，用 `&` 分割。
我们知道，解析报文的过程是通过获取 TCP 数据，用正则等工具从数据中获取 Header 和 Body，从而提取参数。
也就是说，我们可以自己约定参数的写法，只要服务端能够解释出来就行，一种比较流行的写法是 `http://www.example.com/user/name/chengqm/age/22`。
#### 2. POST 方法比 GET 方法安全？
按照网上大部分文章的解释，POST 比 GET 安全，因为数据在地址栏上不可见。
然而，从传输的角度来说，他们都是不安全的，因为 HTTP 在网络上是明文传输的，只要在网络节点上捉包，就能完整地获取数据报文。
要想安全传输，就只有加密，也就是 HTTPS。
#### 3. GET 方法的长度限制是怎么回事？
在网上看到很多关于两者区别的文章都有这一条，提到浏览器地址栏输入的参数是有限的。
首先说明一点，HTTP 协议没有 Body 和 URL 的长度限制，对 URL 限制的大多是浏览器和服务器的原因。
浏览器原因就不说了，服务器是因为处理长 URL 要消耗比较多的资源，为了性能和安全（防止恶意构造长 URL 来攻击）考虑，会给 URL 长度加限制。
#### 4. POST 方法会产生两个TCP数据包？
有些文章中提到，post 会将 header 和 body 分开发送，先发送 header，服务端返回 100 状态码再发送 body。
HTTP 协议中没有明确说明 POST 会产生两个 TCP 数据包，而且实际测试(Chrome)发现，header 和 body 不会分开发送。
所以，header 和 body 分开发送是部分浏览器或框架的请求方法，不属于 post 必然行为。

## 11. 跨域
**什么是同源策略**？
同源策略/`SOP（Same origin policy）`是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到`XSS`、`CSFR`等攻击。所谓同源是指"协议+域名+端口"三者相同，即便两个不同的域名指向同一个ip地址，也非同源。

同源策略限制以下几种行为：
1.) `Cookie、LocalStorage` 和 `IndexDB` 无法读取
2.) `DOM` 和 `Js`对象无法获得
3.) `AJAX` 请求不能发送

可以跨域的三个标签：
- `<img>`
- `<link>`
- `<scirpt>`

### 跨域解决方案
1、 通过jsonp跨域
2、 `document.domain + iframe`跨域
3、 `location.hash + iframe`
4、 `window.name + iframe`跨域
5、 `postMessage`跨域
6、 跨域资源共享（CORS）
7、 `nginx`代理跨域
8、 `nodejs`中间件代理跨域
9、 `WebSocket`协议跨域

**简单的跨域请求jsonp即可，复杂的cors，窗口之间JS跨域postMessage，开发环境下接口跨域用nginx反向代理或node中间件比较方便。**

#### I、 通过jsonp跨域
JSONP实现原理：先定义一个函数，请求外域的API，返回一个JS片段，然后执行
eg:jquery ajax
```js
$.ajax({
    url: 'http://www.domain2.com:8080/login',
    type: 'get',
    dataType: 'jsonp',  // 请求方式为jsonp
    jsonpCallback: "onBack",    // 自定义回调函数名
    data: {}
});
```

#### II、 document.domain + iframe跨域
此方案仅限主域相同，子域不同的跨域应用场景。


#### Nodejs中间件代理跨域
vue框架的跨域（1次跨域）
利用node + webpack + webpack-dev-server代理接口跨域。在开发环境下，由于vue渲染服务和接口代理服务都是webpack-dev-server同一个，所以页面与代理接口之间不再跨域，无须设置headers跨域信息了。

webpack.config.js部分配置：
```javascript {.line-numbers}
module.exports = {
    entry: {},
    module: {},
    ...
    devServer: {
        historyApiFallback: true,
        proxy: [{
            context: '/login',
            target: 'http://www.domain2.com:8080',  // 代理跨域目标接口
            changeOrigin: true,
            secure: false,  // 当代理某些https服务报错时用
            cookieDomainRewrite: 'www.domain1.com'  // 可以为false，表示不修改
        }],
        noInfo: true
    }
}
```
