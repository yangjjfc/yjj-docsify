<!--
 * @Author: yangjj
 * @Date: 2019-08-13 09:02:12
 * @LastEditors: yangjj
 * @LastEditTime: 2019-08-13 13:51:52
 * @Description: file content
 -->
## 事件机制
* js是一个单线程执行,它是基于事件循环的
* 其中事件分为2部分,宏任务(`macroTask`)和微任务(`microTask`)
* 所有的同步任务都在主线程上执行,形成一个执行栈
* 主线程之外,还有一个任务队列,只要有异步任务有了运行结果就会在任务队列中放置一个事件
* 一旦执行栈所有的同步任务完成,系统就会读取任务队列,看看有哪些事件,哪些对应的异步任务,于是结束等待状态,进入执行栈,开始执行
* 期间又产生微任务,就会放入执行栈末尾,立即等待执行,产生宏任务的话,下一轮事件循环执行(猜的~),
* 当任务全部执行完后,会进行UI渲染
* 主线程不断重复上面的步骤

* macroTask: `setTimeout、MessageChannel、postMessage、setImmediate`
* microTask: `MutationObsever、 Promise.then、process.nextTick` 
* [Javascript事件循环机制以及渲染引擎何时渲染UI](https://segmentfault.com/a/1190000013212944)


## nextTick解析

* 执行`src/core/util/next-tick.js`文件,暴露2个函数`nextTick`和`withMacroTask`
* macro-task判断是否支持setImmediate,如果不支持,就看MessageChannel,不支持就降级为 setTimeout 0
* micro-task判断是否支持Promise,不能就使用macro-task
* 通过`microTimerFunc`和`macroTimerFunc `2个变量来分别保存各自任务
* nextTick函数执行时,会把cb函数压入到一个callbacks 数组中,保证在同一个 tick 内多次执行 nextTick，不会开启多个异步任务,而把这些异步任务都压成一个同步任务，在下一个 tick 执行完毕。
* 最后一次性地根据 `useMacroTask` 条件执行 `macroTimerFunc` 或者是 `microTimerFunc，而它们都会在下一个` tick 执行 `flushCallbacks，flushCallbacks` 的逻辑非常简单，对 callbacks 遍历，然后执行相应的回调函数。
* `nextTick` 如果不传cb,也可以支持Promise的方法,在.then中执行函数
* `withMacroTask` 它是对函数做一层包装，确保函数执行过程中对数据任意的修改，触发变化执行 nextTick 的时候强制走 macroTimerFunc。比如对于一些 DOM 交互事件，如 v-on 绑定的事件回调函数的处理，会强制走 macro task。
* [由nextTick原理引出的js执行机制](https://www.cnblogs.com/zjjDaily/p/10478634.html)
* [[vue源码][nextTick]原理以及源码解析](https://juejin.im/post/5d519abce51d453b753a1a9d?utm_source=gold_browser_extension)
* [Vue 的 NextTick](https://510team.github.io/vue/nextTick.html#%E5%88%9D%E7%9C%8B-event-loop)

## 响应式原理
* 双向绑定原理是利用数据劫持,结合发布者-订阅者模式的方式,通过`Object.defineProperty`来劫持各个属性setter、getter,在数据发生变动时发布消息给订阅者，触发响应的回调函数;
* 所谓的订阅者，就像我们在日常生活中，订阅报纸一样。我们订阅报纸的时候，通常都得需要在报社或者一些中介机构进行注册。当有新版的报纸发刊的时候，邮递员就需要向订阅该报纸的人，依次发放报纸
* 在initState时就会对data里面的数据做初始化数据劫持
* 实现一个监听器Observer，用来劫持并监听所有属性，如果有变动的，就通知订阅者。
* 实现一个订阅者Watcher，可以收到属性的变化通知并执行相应的函数，从而更新视图。
* 实现一个订阅器Dep，收集所有的订阅者。
* 实现一个解析器Compile，可以扫描和解析每个节点的相关指令，并根据初始化模板数据以及初始化相应的
* [vue实现原理](https://blog.csdn.net/weixin_37861326/article/details/80854763)
* vue3.0会使用proxy代替defineProperty ,优点不需要深度遍历监听,数组变化也能监听到

## 生命周期
* initLifecycle/Event，往vm上挂载各种属性
* beforeCreated: 实例刚创建
* initInjection/initState: 初始化注入和 data 响应性
* created: 创建完成，属性已经绑定， 但还未生成真实dom
* 进行元素的挂载： $el / vm.$mount()
* 是否有template: 解析成render function
* beforeMount: 模板编译/挂载之前
* 执行render function，生成真实的dom，并替换到dom tree中
* mounted: 组件已挂载
* update: 执行diff算法，比对改变是否需要触发UI更新
* actived / deactivated(keep-alive): 不销毁，缓存，组件激活与失活 
* beforeDestroy: 销毁开始
* remove(): 删除节点
* watcher.teardown(): 清空依赖
* vm.$off(): 解绑监听
* destroyed: 完成后触发钩子