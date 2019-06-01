# VUE
1、active-class是哪个组件的属性？嵌套路由怎么定义？  
答：vue-router模块的router-link组件。
 
2、怎么定义vue-router的动态路由？怎么获取传过来的动态参数？Vue的路由实现？   
答：在router目录下的index.js文件中，对path属性加上/:id。   
    使用router对象的params.id  
**Vue的路由实现：hash模式 和 history模式**  
- hash模式：在浏览器中符号“#”，#以及#后面的字符称之为hash，用window.location.hash读取；  
特点：hash虽然在URL中，但不被包括在HTTP请求中；用来指导浏览器动作，对服务端安全无用，hash不会重加载页面。
hash 模式下，仅 hash 符号之前的内容会被包含在请求中，如 http://www.xxx.com，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 404 错误。  
- history模式：history采用HTML5的新特性；且提供了两个新方法：pushState（），replaceState（）可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更。  
特点：history 模式下，前端的 URL 必须和实际向后端发起请求的 URL 一致，如 http://www.xxx.com/items/id。后端如果缺少对 /items/id 的路由处理，将返回 404 错误。Vue-Router 官网里如此描述：“不过这种模式要玩好，还需要后台配置支持……所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。”

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
答：vue框架中状态管理，当你打算开发大型单页应用（`SPA`），会出现多个视图组件依赖同一个状态，来自不同视图的行为需要变更同一个状态。  
在main.js引入store，注入。新建了一个目录store，….. export 。  
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

12、`Vuex`是什么？为什么使用`Vuex`？
答：`Vuex` 类似 `Redux` 的状态管理器，用来管理Vue的所有组件状态。

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

**js实现简单的双向绑定**
```js
<body>
  <div id="app">
  <input type="text" id="txt">
  <p id="show"></p>
</div>
</body>
<script type="text/javascript">
  var obj = {}
  Object.defineProperty(obj, 'txt', {
    get: function () {
      return obj
    },
    set: function (newValue) {
      document.getElementById('txt').value = newValue
      document.getElementById('show').innerHTML = newValue
    }
  })
  document.addEventListener('keyup', function (e) {
    obj.txt = e.target.value
  })
</script>
```
17、请详细说下你对`vue`生命周期的理解？  
答：`beforeCreate`（**创建前**） 在数据观测和初始化事件还未开始  
`created`（**创建后**） 完成数据观测，属性和方法的运算，初始化事件，$el属性还没有显示出来  
`beforeMount`（**载入前**） 在挂载开始之前被调用，相关的render函数首次被调用。实例已完成以下的配置：编译模板，把data里面的数据和模板生成html。注意此时还没有挂载html到页面上。  
`mounted`（**载入后**） 在el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用。实例已完成以下的配置：用上面编译好的html内容替换el属性指向的DOM对象。完成模板中的html渲染到html页面中。此过程中进行ajax交互。  
`beforeUpdate`（**更新前**） 在数据更新之前调用，发生在虚拟DOM重新渲染和打补丁之前。可以在该钩子中进一步地更改状态，不会触发附加的重渲染过程。  
`updated`（**更新后**） 在由于数据更改导致的虚拟DOM重新渲染和打补丁之后调用。调用时，组件DOM已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。  
`beforeDestroy`（**销毁前**） 在实例销毁之前调用。实例仍然完全可用。  
`destroyed`（**销毁后**） 在实例销毁之后调用。调用后，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用。
- 什么是vue生命周期？  
答： Vue 实例从创建到销毁的过程，就是生命周期。从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、销毁等一系列过程，称之为 Vue 的生命周期。

- vue生命周期的作用是什么？  
答：它的生命周期中有多个事件钩子，让我们在控制整个Vue实例的过程时更容易形成好的逻辑。

- vue生命周期总共有几个阶段？  
答：它可以总共分为8个阶段：创建前/后, 载入前/后,更新前/后,销毁前/销毁后。

- 第一次页面加载会触发哪几个钩子？  
答：会触发 下面这几个beforeCreate, created, beforeMount, mounted 。

- DOM 渲染在 哪个周期中就已经完成？  
答：DOM 渲染在 mounted 中就已经完成了。

18、请说下封装 vue 组件的过程？  
答：首先，组件可以提升整个项目的开发效率。能够把页面抽象成多个相对独立的模块，解决了我们传统项目开发：效率低、难维护、复用性等问题。  
然后，使用Vue.extend方法创建一个组件，然后使用`Vue.component`方法注册组件。子组件需要数据，可以在`props`中接受定义。而子组件修改好数据后，想把数据传递给父组件。可以采用`emit`方法。

19、mvvm框架是什么？它和其它框架（`jquery`）的区别是什么？哪些场景适合？  
答：MVVM 是 `Model-View-ViewModel` 的缩写。  
`Model`代表数据模型，也可以在`Model`中定义数据修改和操作的业务逻辑。  
`View` 代表`UI` 组件，它负责将数据模型转化成`UI` 展现出来。  
`ViewModel` 监听模型数据的改变和控制视图行为、处理用户交互，简单理解就是一个同步`View` 和 `Model`的对象，连接`Model`和`View`。
在`MVVM`架构下，`View` 和 `Model` 之间并没有直接的联系，而是通过`ViewModel`进行交互，`Model` 和 `ViewModel` 之间的交互是双向的， 因此`View` 数据的变化会同步到`Model`中，而`Model` 数据的变化也会立即反应到`View` 上。  
`ViewModel` 通过双向数据绑定把 `View` 层和 `Model` 层连接了起来，而`View` 和 `Model` 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 `MVVM` 来统一管理。
 
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

24、自定义指令（`v-check`、`v-focus`）的方法有哪些？它有哪些钩子函数？还有哪些钩子函数参数？  
答：全局定义指令：在vue对象的`directive`方法里面有两个参数，一个是指令名称，另外一个是函数。  
组件内定义指令：`directives`  
钩子函数：`bind`（绑定事件触发）、`inserted`(节点插入的时候触发)、`update`（组件内相关更新）  
钩子函数参数：`el`、`binding`

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
- 1.父组件与子组件传值  
父组件传给子组件：子组件通过props方法接受数据;
子组件传给父组件：$emit方法传递参数
- 2.非父子组件间的数据传递，兄弟组件传值  
`eventBus`，就是创建一个事件中心，相当于中转站，可以用它来传递事件和接收事件。项目比较小时，用这个比较合适。（虽然也有不少人推荐直接用`VUEX`，具体来说看需求咯。技术只是手段，目的达到才是王道。）

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

32、vue-cli如何新增自定义指令？
1.创建局部指令
```js
var app = new Vue({
  el: '#app',
  data: {},
  // 创建指令(可以多个)
  directives: {
    // 指令名称
    dir1: {
      inserted(el) {
        // 指令中第一个参数是当前使用指令的DOM
        console.log(el);
        console.log(arguments);
        // 对DOM进行操作
        el.style.width = '200px';
        el.style.height = '200px';
        el.style.background = '#000';
      }
    }
  }
})
```
2.全局指令
```js
Vue.directive('dir2', {
  inserted(el) {
      console.log(el);
  }
})
```
3.指令的使用
```js
<div id="app">
  <div v-dir1></div>
  <div v-dir2></div>
</div>
```

33、 一句话就能回答的面试题  
1.css只在当前组件起作用  
答：在style标签中写入scoped即可 例如：<style scoped></style>

2.v-if 和 v-show 区别  
答：v-if按照条件是否渲染，v-show是display的block或none；

3.$route和$router的区别  
答：$route是“路由信息对象”，包括path，params，hash，query，fullPath，matched，name等路由信息参数。而$router是“路由实例”对象包括了路由的跳转方法，钩子函数等。

4.vue.js的两个核心是什么？  
答：数据驱动、组件系统

5.vue几种常用的指令  
答：v-for 、 v-if 、v-bind、v-on、v-show、v-else

6.vue常用的修饰符？  
答：.prevent: 提交事件不再重载页面；.stop: 阻止单击事件冒泡；.self: 当事件发生在该元素本身而不是子元素的时候会触发；.capture: 事件侦听，事件发生的时候会调用

7.v-on 可以绑定多个方法吗？  
答：可以

8.vue中 key 值的作用？  
答：当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。key的作用主要是为了高效的更新虚拟DOM。

9.什么是vue的计算属性？  
答：在模板中放入太多的逻辑会让模板过重且难以维护，在需要对数据进行复杂处理，且可能多次使用的情况下，尽量采取计算属性的方式。好处：①使得数据处理结构清晰；②依赖于数据，数据更新，处理结果自动更新；③计算属性内部this指向vm实例；④在template调用时，直接写计算属性名即可；⑤常用的是getter方法，获取数据，也可以使用set方法改变数据；⑥相较于methods，不管依赖的数据变不变，methods都会重新计算，但是依赖数据不变的时候computed从缓存中获取，不会重新计算。

10.vue等单页面应用及其优缺点  
答：优点：Vue 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件，核心是一个响应的数据绑定系统。MVVM、数据驱动、组件化、轻量、简洁、高效、快速、模块友好。  
缺点：不支持低版本的浏览器，最低只支持到IE9；不利于SEO的优化（如果要支持SEO，建议通过服务端来进行渲染组件）；第一次加载首页耗时相对长一些；不可以使用浏览器的导航按钮需要自行实现前进、后退。

11.Vuejs中关于computed、methods、watch的区别  
- `computed`：计算属性将被混入到 Vue 实例中。所有 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例。
- `methods`：methods 将被混入到 Vue 实例中。可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 this 自动绑定为 Vue 实例。
- `watch`：是一种更通用的方式来观察和响应 Vue 实例上的数据变动。一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。Vue 实例将会在实例化时调用 $watch()，遍历 watch 对象的每一个属性。

通俗来讲，
computed是在HTML DOM加载后马上执行的，如赋值；  
而methods则必须要有一定的触发条件才能执行，如点击事件；  
watch呢？它用于观察Vue实例上的数据变动。对应一个对象，键是观察表达式，值是对应回调。值也可以是方法名，或者是对象，包含选项。  
所以他们的执行顺序为：默认加载的时候先computed再watch，不执行methods；等触发某一事件后，则是：先methods再watch。

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
## Vue与Angular以及React的区别？
|`VUE`|`angular`|
|:----|:----|
|v-if|*ngIf|
|v-for="item in items"|*ngForr="let hero of heroes"|
|v-on:click="fn()"|(click)="fn()"|
|v-bind:class="{ active: isActive }"|[class.selected]="flage == true"|
都支持模板语法:  `<h1>{{title}}</h1>`

（版本在不断更新，以下的区别有可能不是很正确。我工作中只用到vue，对angular和react不怎么熟）  
1.与`AngularJS`的区别  
相同点：  
都支持指令：内置指令和自定义指令；都支持过滤器：内置过滤器和自定义过滤器；都支持双向数据绑定；都不支持低端浏览器。

不同点：  
`AngularJS`的学习成本高，比如增加了`Dependency Injection`特性，而Vue.js本身提供的API都比较简单、直观；在性能上，`AngularJS`依赖对数据做脏检查，所以`Watcher`越多越慢；Vue.js使用基于依赖追踪的观察并且使用异步队列更新，所有的数据都是独立触发的。

2.与`React`的区别  
相同点：  
`React`采用特殊的`JSX`语法，`Vue.js`在组件开发中也推崇编写`.vue`特殊文件格式，对文件内容都有一些约定，两者都需要编译后使用；  
中心思想相同：  
一切都是组件，组件实例之间可以嵌套；都提供合理的钩子函数，可以让开发者定制化地去处理需求；  
都不内置列数`AJAX`，`Route`等功能到核心包，而是以插件的方式加载；在组件开发中都支持mixins的特性。  
不同点：  
`React`采用的`Virtual DOM`会对渲染出来的结果做脏检查；Vue.js在模板中提供了指令，过滤器等，可以非常方便，快捷地操作`Virtual DOM`。
