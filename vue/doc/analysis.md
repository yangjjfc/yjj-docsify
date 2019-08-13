<!--
 * @Author: yangjj
 * @Date: 2019-08-13 09:02:12
 * @LastEditors: yangjj
 * @LastEditTime: 2019-08-13 09:37:30
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
* [由nextTick原理引出的js执行机制](https://www.cnblogs.com/zjjDaily/p/10478634.html)
* [[vue源码][nextTick]原理以及源码解析](https://juejin.im/post/5d519abce51d453b753a1a9d?utm_source=gold_browser_extension)
## nextTick解析



