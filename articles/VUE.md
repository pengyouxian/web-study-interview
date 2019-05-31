# VUE

## vue和angular的一些对比：
|`VUE`|`angular`|
|:----|:----|
|v-if|*ngIf|
|v-for="item in items"|*ngForr="let hero of heroes"|
|v-on:click="fn()"|(click)="fn()"|
|v-bind:class="{ active: isActive }"|[class.selected]="flage == true"|
都支持模板语法:  `<h1>{{title}}</h1>`

1、active-class是哪个组件的属性？嵌套路由怎么定义？  
答：vue-router模块的router-link组件。
 
2、怎么定义vue-router的动态路由？怎么获取传过来的动态参数？   
答：在router目录下的index.js文件中，对path属性加上/:id。   
    使用router对象的params.id

3、vue-router有哪几种导航钩子？    
答：三种，   
第一种是全局导航钩子：router.beforeEach(to,from,next)，作用：跳转前进行判断拦截。  
第二种：组件内的钩子；  
第三种：单独路由独享组件

4、`vue-router`是什么？它有哪些组件？  
答：vue用来写路由一个插件。`router-link`、`router-view`
- **mode**
    - `hash`
    - `history`
- **跳转**
    - `this.$router.push()`
    - `<router-link to=""></router-link>`
- **占位**
    - `<router-view></router-view>`

5、mint-ui是什么？怎么使用？说出至少三个组件使用方法？  
答：基于vue的前端组件库。  
npm安装，然后import样式和js，vue.use（mintUi）全局引入。  
在单个组件局部引入：`import {Toast} from 'mint-ui'`。  
组件一：`Toast('登录成功')`；  
组件二：`mint-header`；  
组件三：`mint-swiper`

6、v-model是什么？怎么使用？ vue中标签怎么绑定事件？  
答：可以实现双向绑定，指令（`v-class`、`v-for`、`v-if`、`v-show`、`v-on`）。  
vue的model层的data属性。绑定事件：`<input @click=doLog() />`

7、axios是什么？怎么使用？描述使用它实现登录功能的流程？  
答：请求后台资源的模块。  
`npm install axios -S`装好，然后发送的是跨域，需在配置文件中config/index.js进行设置。  
后台如果是Tp5则定义一个资源路由。js中使用import进来，然后.get或.post。返回在.then函数中如果成功，失败则是在.catch函数中

8、axios+tp5进阶中，调用`axios.post('api/user')`是进行的什么操作？`axios.put('api/user/8′)`呢？  
答：跨域，添加用户操作，更新操作。

9、什么是RESTful API？怎么使用?  
答：是一个api的标准，无状态请求。请求的路由地址是固定的，如果是tp5则先路由配置中把资源路由配置好。  
标准有：.post .put .delete
 

10、vuex是什么？怎么使用？哪种功能场景使用它？  
答：vue框架中状态管理。在main.js引入store，注入。新建了一个目录store，….. export 。  
场景有：单页应用中，组件之间的状态。音乐播放、登录状态、加入购物车
- `state`: 状态中心
- `mutations`: 更改状态
- `actions`: 异步更改状态
- `getters`: 获取状态
- `modules`: 将`state`分成多个`modules`，便于管理

11、你是怎么认识vuex的？  
答：vuex可以理解为一种开发模式或框架。比如PHP有thinkphp，java有spring等。  
通过状态（数据源）集中管理驱动组件的变化（好比spring的IOC容器对bean进行集中管理）。  
应用级的状态集中放在store中； 改变状态的方式是提交mutations，这是个同步的事物； 异步逻辑应该封装在action中。

12、自定义指令（`v-check`、`v-focus`）的方法有哪些？它有哪些钩子函数？还有哪些钩子函数参数？  
答：全局定义指令：在vue对象的`directive`方法里面有两个参数，一个是指令名称，另外一个是函数。  
组件内定义指令：`directives`  
钩子函数：`bind`（绑定事件触发）、`inserted`(节点插入的时候触发)、`update`（组件内相关更新）  
钩子函数参数：`el`、`binding`
 
13、说出至少4种vue当中的指令和它的用法？  
答：`v-if`：判断是否隐藏；`v-for`：数据循环出来；  
`v-bind:class`：绑定一个属性；`v-model`：实现双向绑定

14、scss是什么？安装使用的步骤是？有哪几大特性？  
答：预处理css，把css当前函数编写，定义变量,嵌套。   
先装css-loader、node-loader、sass-loader等加载器模块，  
在webpack-base.config.js配置文件中加多一个拓展:extenstion，  
再加多一个模块：module里面test、loader

4.1、scss是什么？在vue.cli中的安装使用步骤是？有哪几大特性？  
答：css的预编译。

使用步骤：
第一步：用npm 下三个loader（sass-loader、css-loader、node-sass）  
第二步：在build目录找到webpack.base.config.js，在那个extends属性中加一个拓展.scss  
第三步：还是在同一个文件，配置一个module属性  
第四步：然后在组件的style标签加上lang属性 ，例如：lang=”scss”  

有哪几大特性:  
1、可以用变量，例如（$变量名称=值）；  
2、可以用混合器，例如（）  
3、可以嵌套  
 
15、导航钩子有哪些？它们有哪些参数？  
答：导航钩子有：a/全局钩子和组件内独享的钩子。  
b/beforeRouteEnter、afterEnter、beforeRouterUpdate、beforeRouteLeave

参数：有to（去的那个路由）、from（离开的路由）、  
next（一定要用这个函数才能去到下一个路由，如果不用就拦截）最常用就这几种

16、Vue的双向数据绑定原理是什么？  
答：vue.js 是采用数据劫持结合`发布者-订阅者模式`的方式，通过`Object.defineProperty()`来劫持各个属性的`setter`，`getter`，在数据变动时发布消息给订阅者，触发相应的监听回调。  
具体步骤：  
第一步：需要`observe`的数据对象进行递归遍历，包括子属性对象的属性，都加上 `setter`和`getter`
这样的话，给这个对象的某个值赋值，就会触发`setter`，那么就能监听到了数据变化  
第二步：`compile`解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图  
第三步：`Watcher`订阅者是`Observer`和`Compile`之间通信的桥梁，主要做的事情是:  
1、在自身实例化时往属性订阅器(dep)里面添加自己  
2、自身必须有一个`update()`方法  
3、待属性变动`dep.notice()`通知时，能调用自身的`update()`方法，并触发Compile中绑定的回调，则功成身退。  
第四步：`MVVM`作为数据绑定的入口，整合`Observer`、`Compile`和`Watcher`三者，通过`Observer`来监听自己的`model`数据变化，通过`Compile`来解析编译模板指令，最终利用`Watcher`搭起`Observer`和`Compile`之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化`(input)` -> 数据`model`变更的双向绑定效果。

ps：16题答案同样适合”vue data是怎么实现的？”此面试题。

17、请详细说下你对`vue`生命周期的理解？  
答：总共分为8个阶段**创建**前/后，**载入**前/后，**更新**前/后，**销毁**前/后。  
**创建前/后**： 在`beforeCreated`阶段，`vue`实例的挂载元素`$el`和数据对象`data`都为`undefined`，还未初始化。在`created`阶段，`vue`实例的数据对象`data`有了，`$el`还没有。  
**载入前/后**：在`beforeMount`阶段，`vue`实例的`$el`和`data`都初始化了，但还是挂载之前为虚拟的`dom`节点，`data.message`还未替换。在`mounted`阶段，`vue`实例挂载完成，`data.message`成功渲染。  
**更新前/后**：当`data`变化时，会触发`beforeUpdate`和`updated`方法。  
**销毁前/后**：在执行`destroy`方法后，对`data`的改变不会再触发周期函数，说明此时`vue`实例已经解除了事件监听以及和`dom`的绑定，但是`dom`结构依然存在.

18、请说下封装 vue 组件的过程？  
答：首先，组件可以提升整个项目的开发效率。能够把页面抽象成多个相对独立的模块，解决了我们传统项目开发：效率低、难维护、复用性等问题。  
然后，使用Vue.extend方法创建一个组件，然后使用`Vue.component`方法注册组件。子组件需要数据，可以在`props`中接受定义。而子组件修改好数据后，想把数据传递给父组件。可以采用`emit`方法。

19、mvvm框架是什么？它和其它框架（`jquery`）的区别是什么？哪些场景适合？
答：一个`model+view+viewModel`框架，数据模型`model`，`viewModel`连接两个  
区别：vue数据驱动，通过数据来显示视图层而不是节点操作。  
场景：数据操作比较多的场景，更加便捷  
 
20、vue-loader是什么？使用它的用途有哪些？  
答：解析.vue文件的一个加载器，跟template/js/style转换成js模块。  
用途：js可以写es6、style样式可以scss或less、template可以加jade等

21、请说出vue.cli项目中src目录每个文件夹和文件的用法？  
答：assets文件夹是放静态资源；  
components是放组件；  
router是定义路由相关的配置;  
view视图；  
app.vue是一个应用主组件；  
main.js是入口文件

22、vue.cli中怎样使用自定义的组件？有遇到过哪些问题吗？  
答：第一步：在`components`目录新建你的组件文件（`smithButton.vue`），script一定要`export default {`  
第二步：在需要用的页面（组件）中导入：`import smithButton from '../components/smithButton.vue'`  
第三步：注入到`vue`的子组件的`components`属性上面,`components:{smithButton}`  
第四步：在`template`视图view中使用，`<smith-button>  </smith-button>`
问题有：`smithButton`命名，使用的时候则`smith-button`。  
 

23、聊聊你对Vue.js的`template`编译的理解？  
答：简而言之，就是先转化成AST树，再得到的render函数返回VNode（`Vue`的虚拟`DOM`节点）  
详情步骤：  
首先，通过`compile`编译器把`template`编译成`AST`语法树（abstract syntax tree 即 源代码的抽象语法结构的树状表现形式），`compile`是`createCompiler`的返回值，`createCompiler`是用以创建编译器的。另外`compile`还负责合并`option`。
然后，`AST`会经过`generate`（将AST语法树转化成render funtion字符串的过程）得到`render`函数，`render`的返回值是`VNode`，`VNode`是`Vue`的虚拟`DOM`节点，里面有（标签名、子节点、文本等等）

24、`Vuex`是什么？为什么使用`Vuex`？
答：`Vuex` 类似 `Redux` 的状态管理器，用来管理Vue的所有组件状态。
当你打算开发大型单页应用（`SPA`），会出现多个视图组件依赖同一个状态，来自不同视图的行为需要变更同一个状态。

25、vuejs与angularjs的区别？  
答：  
一、定位 ：  
虽然Vue.js被定义为MVC framework，但其实Vue本身还是一个library，加了一些其他的工具，可以被当成一个framework，而Angular 2虽然还是一个framework，但其实在设计之初，Angular 2的团队站在了更高的角度，希望做一个platform。

二、文档：  
vue.js的更加亲切

三、性能：  
angular所有的数据和方法都是挂载在$scope上。而vue的数据和方法都是挂载在vue上，只是数据挂载在vue的data,方法挂载在vue的methods上，vue的代码风格更加优雅，json格式书写代码。Vue.js 有更好的性能，并且非常非常容易优化，因为它不使用脏检查。Angular，当 watcher 越来越多时会变得越来越慢，因为作用域内的每一次变化，所有 watcher 都要重新计算。

其它区别：  
渲染性能：Vue> react >angular。  
使用场景：Vue React 覆盖中小型，大型项目。angular 一般用于大型（因为比较厚重）。

26、vue为什么不直接操作dom？  
答：因为操作dom对象后，会触发一些浏览器行为，比如布局（layout）和绘制（paint）。  
paint是一个耗时的过程，然而layout是一个更耗时的过程，我们无法确定layout一定是自上而下或是自下而上进行的，甚至一次layout会牵涉到整个文档布局的重新计算。浏览器的layout是lazy的，也就是说：在js脚本执行时，是不会去更新DOM的，任何对DOM的修改都会被暂存在一个队列中，在当前js的执行上下文完成执行后，会根据这个队列中的修改，进行一次layout。

27、你怎么理解`vue`是一个渐进式的框架？  
答：我觉得渐进式就是不必一开始就用`Vue`所有的全家桶，可以根据场景，按需使用想要的插件。也可以说就使用`vue`不需要太多的要求。

28、`Vue`声明组件的`state`是用`data`方法，那为什么`data`是通过一个`function`来返回一个对象，而不是直接写一个对象呢？  
答：从语法上说，如果不用`function`返回就会出现语法错误导致编译不通过。从原理上的话，大概就是组件可以被多次创建，如果不使用`function`就会使所有调用该组件的页面公用同一个数据域，这样就失去了组件的概念了


29、说下vue组件之间的通信？  
答：非父子组件间通信，Vue 有提供 Vuex，以状态共享方式来实现同信，对于这一点，应该注意考虑平衡，从整体设计角度去考量，确保引入她的必要。  
父传子: `this.$refs.xxx`   
子传父: `this.$parent.xxx`  
还可以通过`$emit`方法出发一个消息，然后`$on`接收这个消息

30、vue中mixin与extend区别？  
答：全局注册混合对象，会影响到所有之后创建的vue实例，而Vue.extend是对单个实例进行扩展。  
    mixin 混合对象（组件复用）
同名钩子函数（`bind`，`inserted`，`update`，`componentUpdate`，`unbind`）将混合为一个数组，因此都将被调用，混合对象的钩子将在组件自身钩子之前调用
`methods`，`components`，`directives`将被混为同一个对象。两个对象的键名（方法名，属性名）冲突时，取组件（而非mixin）对象的键值对

31、 nextTick
在下次`dom`更新循环结束之后执行延迟回调，可用于获取更新后的`dom`状态
- 新版本中默认是`mincrotasks`, `v-on`中会使用`macrotasks`
- `macrotasks`任务的实现:
    - `setImmediate / MessageChannel / setTimeout`

## Proxy 相比于 defineProperty 的优势
- 数组变化也能监听到
- 不需要深度遍历监听
```js {.line-numbers}
let data = { a: 1 }
let reactiveData = new Proxy(data, {
    get: function(target, name){
        // ...
    },
    // ...
})
```

