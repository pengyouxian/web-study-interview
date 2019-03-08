# VUE

## vue和angular的一些对比：
|`VUE`|`angular`|
|:----|:----|
|v-if|*ngIf|
|v-for="item in items"|*ngForr="let hero of heroes"|
|v-on:click="fn()"|(click)="fn()"|
|v-bind:class="{ active: isActive }"|[class.selected]="flage == true"|
都支持模板语法:  `<h1>{{title}}</h1>`

## 1. nextTick
在下次`dom`更新循环结束之后执行延迟回调，可用于获取更新后的`dom`状态
- 新版本中默认是`mincrotasks`, `v-on`中会使用`macrotasks`
- `macrotasks`任务的实现:
    - `setImmediate / MessageChannel / setTimeout`

## 2. 生命周期
- `_init_`
    - `initLifecycle/Event`，往`vm`上挂载各种属性
    - `callHook`: `beforeCreated`: 实例刚创建
    - `initInjection`/`initState`: 初始化注入和 data 响应性
    - `created`: 创建完成，属性已经绑定， 但还未生成真实dom
    - 进行元素的挂载： `$el / vm.$mount()`
    - 是否有`template`: 解析成`render function`
        - `*.vue`文件: `vue-loader`会将`<template>`编译成`render function` 
    - `beforeMount`: 模板编译/挂载之前
    -  执行`render function`，生成真实的`dom`，并替换到`dom tree`中
    - `mounted`: 组件已挂载
- `update`:
    - 执行`diff`算法，比对改变是否需要触发UI更新
    - `flushScheduleQueue`
        - `watcher.before`: 触发`beforeUpdate`钩子 
        - `watcher.run()`: 执行`watcher`中的 notify，通知所有依赖项更新UI
    - 触发`updated`钩子: 组件已更新
- `actived / deactivated(keep-alive)`: 不销毁，缓存，组件激活与失活
- `destroy`:
    - `beforeDestroy`: 销毁开始- 
    - 销毁自身且递归销毁子组件以及事件监听
        - `remove()`: 删除节点
        - `watcher.teardown()`: 清空依赖
        - `vm.$off()`: 解绑监听
    - `destroyed`: 完成后触发钩子
上面是`vue`的声明周期的简单梳理，接下来直接以代码的形式来完成`vue`的**初始化**:
```js {.line-numbers}
new Vue({})

// 初始化Vue实例
function _init() {
     // 挂载属性
    initLifeCycle(vm) 
    // 初始化事件系统，钩子函数等
    initEvent(vm) 
    // 编译slot、vnode
    initRender(vm) 
    // 触发钩子
    callHook(vm, 'beforeCreate')
    // 添加inject功能
    initInjection(vm)
    // 完成数据响应性 props/data/watch/computed/methods
    initState(vm)
    // 添加 provide 功能
    initProvide(vm)
    // 触发钩子
    callHook(vm, 'created')
        
     // 挂载节点
    if (vm.$options.el) {
        vm.$mount(vm.$options.el)
    }
}

// 挂载节点实现
function mountComponent(vm) {
     // 获取 render function
    if (!this.options.render) {
        // template to render
        // Vue.compile = compileToFunctions
        let { render } = compileToFunctions() 
        this.options.render = render
    }
    // 触发钩子
    callHook('beforeMounte')
    // 初始化观察者
    // render 渲染 vdom， 
    vdom = vm.render()
    // update: 根据 diff 出的 patchs 挂载成真实的 dom 
    vm._update(vdom)
    // 触发钩子  
    callHook(vm, 'mounted')
}

// 更新节点实现
funtion queueWatcher(watcher) {
    nextTick(flushScheduleQueue)
}

// 清空队列
function flushScheduleQueue() {
     // 遍历队列中所有修改
    for(){
        // beforeUpdate
        watcher.before()
         
        // 依赖局部更新节点
        watcher.update() 
        callHook('updated')
    }
}

// 销毁实例实现
Vue.prototype.$destory = function() {
     // 触发钩子
    callHook(vm, 'beforeDestory')
    // 自身及子节点
    remove() 
    // 删除依赖
    watcher.teardown() 
    // 删除监听
    vm.$off() 
    // 触发钩子
    callHook(vm, 'destoryed')
}
```
## 3. 数据响应(数据劫持)
看完生命周期后，里面的`watcher`等内容其实是数据响应中的一部分。数据响应的实现由两部分构成: **观察者( watcher )** 和 **依赖收集器( Dep )**，其核心是 `defineProperty`这个方法，它可以 **重写属性的 get 与 set** 方法，从而完成监听数据的改变。
- **Observe (观察者)**观察 props 与 state
    - 遍历 props 与 state，对每个属性创建独立的监听器( watcher )
- 使用 `defineProperty` 重写每个属性的 get/set(defineReactive）
    - `get`: 收集依赖
        - `Dep.depend()`
            - `watcher.addDep()`
    - `set`: 派发更新
        - `Dep.notify()`
        - `watcher.update()`
        - `queenWatcher()`
        - `nextTick`
        - `flushScheduleQueue`
        - `watcher.run()`
        - `updateComponent()`
大家可以先看下面的数据相应的代码实现后，理解后就比较容易看懂上面的简单脉络了。
```js {.line-numbers}
let data = {a: 1}
// 数据响应性
observe(data)

// 初始化观察者
new Watcher(data, 'name', updateComponent)
data.a = 2

// 简单表示用于数据更新后的操作
function updateComponent() {
    vm._update() // patchs
}

// 监视对象
function observe(obj) {
     // 遍历对象，使用 get/set 重新定义对象的每个属性值
    Object.keys(obj).map(key => {
        defineReactive(obj, key, obj[key])
    })
}

function defineReactive(obj, k, v) {
    // 递归子属性
    if (type(v) == 'object') observe(v)
    
    // 新建依赖收集器
    let dep = new Dep()
    // 定义get/set
    Object.defineProperty(obj, k, {
        enumerable: true,
        configurable: true,
        get: function reactiveGetter() {
              // 当有获取该属性时，证明依赖于该对象，因此被添加进收集器中
            if (Dep.target) {
                dep.addSub(Dep.target)
            }
            return v
        },
        // 重新设置值时，触发收集器的通知机制
        set: function reactiveSetter(nV) {
            v = nV
            dep.nofify()
        },
    })
}

// 依赖收集器
class Dep {
    constructor() {
        this.subs = []
    }
    addSub(sub) {
        this.subs.push(sub)
    }
    notify() {
        this.subs.map(sub => {
            sub.update()
        })
    }
}

Dep.target = null

// 观察者
class Watcher {
    constructor(obj, key, cb) {
        Dep.target = this
        this.cb = cb
        this.obj = obj
        this.key = key
        this.value = obj[key]
        Dep.target = null
    }
    addDep(Dep) {
        Dep.addSub(this)
    }
    update() {
        this.value = this.obj[this.key]
        this.cb(this.value)
    }
    before() {
        callHook('beforeUpdate')
    }
}
```
## 4. virtual dom 原理实现
- 创建 dom 树
- 树的`diff`，同层对比，输出`patchs(listDiff/diffChildren/diffProps)`
    - 没有新的节点，返回
    - 新的节点`tagName`与`key`不变， 对比`props`，继续递归遍历子树
        - 对比属性(对比新旧属性列表):
            - 旧属性是否存在与新属性列表中
            - 都存在的是否有变化
            - 是否出现旧列表中没有的新属性
    - `tagName`和`key`值变化了，则直接替换成新节点
- 渲染差异
    - 遍历`patchs`， 把需要更改的节点取出来
    - 局部更新`dom`
代码实现：
```js {.line-numbers}
// diff算法的实现
function diff(oldTree, newTree) {
     // 差异收集
    let pathchs = {}
    dfs(oldTree, newTree, 0, pathchs)
    return pathchs
}

function dfs(oldNode, newNode, index, pathchs) {
    let curPathchs = []
    if (newNode) {
        // 当新旧节点的 tagName 和 key 值完全一致时
        if (oldNode.tagName === newNode.tagName && oldNode.key === newNode.key) {
              // 继续比对属性差异
            let props = diffProps(oldNode.props, newNode.props)
            curPathchs.push({ type: 'changeProps', props })
            // 递归进入下一层级的比较
            diffChildrens(oldNode.children, newNode.children, index, pathchs)
        } else {
              // 当 tagName 或者 key 修改了后，表示已经是全新节点，无需再比
            curPathchs.push({ type: 'replaceNode', node: newNode })
        }
    }

     // 构建出整颗差异树
    if (curPathchs.length) {
            if(pathchs[index]){
                pathchs[index] = pathchs[index].concat(curPathchs)
            } else {
                pathchs[index] = curPathchs
            }
    }
}

// 属性对比实现
function diffProps(oldProps, newProps) {
    let propsPathchs = []
    // 遍历新旧属性列表
    // 查找删除项
    // 查找修改项
    // 查找新增项
    forin(olaProps, (k, v) => {
        if (!newProps.hasOwnProperty(k)) {
            propsPathchs.push({ type: 'remove', prop: k })
        } else {
            if (v !== newProps[k]) {
                propsPathchs.push({ type: 'change', prop: k , value: newProps[k] })
            }
        }
    })
    forin(newProps, (k, v) => {
        if (!oldProps.hasOwnProperty(k)) {
            propsPathchs.push({ type: 'add', prop: k, value: v })
        }
    })
    return propsPathchs
}

// 对比子级差异
function diffChildrens(oldChild, newChild, index, pathchs) {
        // 标记子级的删除/新增/移动
    let { change, list } = diffList(oldChild, newChild, index, pathchs)
    if (change.length) {
        if (pathchs[index]) {
            pathchs[index] = pathchs[index].concat(change)
        } else {
            pathchs[index] = change
        }
    }

     // 根据 key 获取原本匹配的节点，进一步递归从头开始对比
    oldChild.map((item, i) => {
        let keyIndex = list.indexOf(item.key)
        if (keyIndex) {
            let node = newChild[keyIndex]
            // 进一步递归对比
            dfs(item, node, index, pathchs)
        }
    })
}

// 列表对比，主要也是根据 key 值查找匹配项
// 对比出新旧列表的新增/删除/移动
function diffList(oldList, newList, index, pathchs) {
    let change = []
    let list = []
    const newKeys = getKey(newList)
    oldList.map(v => {
        if (newKeys.indexOf(v.key) > -1) {
            list.push(v.key)
        } else {
            list.push(null)
        }
    })

    // 标记删除
    for (let i = list.length - 1; i>= 0; i--) {
        if (!list[i]) {
            list.splice(i, 1)
            change.push({ type: 'remove', index: i })
        }
    }

    // 标记新增和移动
    newList.map((item, i) => {
        const key = item.key
        const index = list.indexOf(key)
        if (index === -1 || key == null) {
            // 新增
            change.push({ type: 'add', node: item, index: i })
            list.splice(i, 0, key)
        } else {
            // 移动
            if (index !== i) {
                change.push({
                    type: 'move',
                    form: index,
                    to: i,
                })
                move(list, index, i)
            }
        }
    })

    return { change, list }
}
```
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
## 6. vue-router
- **mode**
    - `hash`
    - `history`
- **跳转**
    - `this.$router.push()`
    - `<router-link to=""></router-link>`
- **占位**
    - `<router-view></router-view>`
## 7. vuex
- `state`: 状态中心
- `mutations`: 更改状态
- `actions`: 异步更改状态
- `getters`: 获取状态
- `modules`: 将`state`分成多个`modules`，便于管理

