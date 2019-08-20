<!--
 * @Author: yangjj
 * @Date: 2019-08-13 09:02:12
 * @LastEditors: yangjj
 * @LastEditTime: 2019-08-19 14:29:20
 * @Description: file content
 -->
## 事件机制
* js是一个单线程执行,它是基于事件循环的
* 其中事件分为2部分,同步任务和异步任务
* 异步任务又分为宏任务(`macroTask`)和微任务(`microTask`),微任务优先级高于宏任务
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
* `withMacroTask` 它是对函数做一层包装，确保函数执行过程中对数据任意的修改，触发变化执行 nextTick 的时候强制走 macroTimerFunc。比如对于一些 DOM 交互事件，如 v-on 绑定的事件回调函数的处理，会强制走 macro task
* 执行完`microtask`后会触发UI渲染,然而执行`macrotask`后会触发`microtask`,会触发多次渲染
* [由nextTick原理引出的js执行机制](https://www.cnblogs.com/zjjDaily/p/10478634.html)
* [[vue源码][nextTick]原理以及源码解析](https://juejin.im/post/5d519abce51d453b753a1a9d?utm_source=gold_browser_extension)
* [Vue 的 NextTick](https://510team.github.io/vue/nextTick.html#%E5%88%9D%E7%9C%8B-event-loop)
* [Vue异步更新 - nextTick为什么要microtask优先](https://juejin.im/post/5d57994ef265da03bd051969?utm_source=gold_browser_extension)

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


## 虚拟DOM
*  浏览器渲染引擎工作流程都差不多，大致分为5步，创建DOM树——创建StyleRules——创建Render树——布局Layout——绘制Painting
* 普通的DOM操作,比如要更新10次,更新一次就要执行上面5个操作,当第一次更新完后,马上第二次又得重新计算更新,很消耗性能
* vdom,就可以把dom转换成js对象,然后10次更新,就操作10次js,js的计算性能很高,等10次更新完后,就是执行diff算法,最后将计算的结果一次性渲染到页面上
*  diff算法,深度优先遍历，记录差异
* 节点类型变了`REPLACE`,直接将旧节点卸载并装载新节点
* 节点类型一样，仅仅属性或属性值变了`PROPS`,此时不会触发节点卸载和装载，而是节点更新
* 文本变了`TEXT`,替换文本
* 移动／增加／删除 子节点`REORDER`,一般遍历就是从左到右依次对比,不同就卸载,然后装载
* 为了执行效率,一般需要加个`key`属性,直接找到具体位置进行操作
* 最后映射成真实DOM
* 我们会有两个虚拟DOM(js对象，new/old进行比较diff)，用户交互我们操作数据变化new虚拟DOM，old虚拟DOM会映射成实际DOM(js对象生成的DOM文档)通过DOM fragment操作给浏览器渲染。当修改new虚拟DOM，会把newDOM和oldDOM通过diff算法比较，得出diff结果数据表(用4种变换情况表示)。再把diff结果表通过DOM fragment更新到浏览器DOM中。
* vdom 的真正意义是为了实现跨平台，服务端渲染，以及提供一个性能还算不错 Dom 更新策略。
* [vue核心之虚拟DOM(vdom)](https://www.jianshu.com/p/af0b398602bc)

## watch和computed区别
* watch:主要是通过一个监听一个属性变化,在数据变化时执行异步或开销较大的操作时,去使用
* computed:依赖的属性发生变化,就会触发更新,一般就用于视图渲染
* computed是计算一个新的属性，并将该属性挂载到vm（Vue实例）上，而watch是监听已经存在且已挂载到vm上的数据，所以用watch同样可以监听computed计算属性的变化（其它还有data、props）
* computed本质是一个惰性求值的观察者，具有缓存性，只有当依赖变化后，第一次访问 computed 属性，才会计算新的值，而watch则是当数据发生变化便会调用执行函数
* 从使用场景上说，computed适用一个数据被多个数据影响，而watch适用一个数据影响多个数据；
* [浅谈Vue中计算属性computed的实现原理](https://segmentfault.com/a/1190000016368913)