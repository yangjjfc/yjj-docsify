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
* 主线程之外,还有一个任务队列,只要有异步任务有了运行结果名酒在任务队列中放置一个事件
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


