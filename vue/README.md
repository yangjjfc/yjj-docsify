# 基础

## 参考系统
[vue-element-admin-site](https://panjiachen.github.io/vue-element-admin-site/zh/guide/)

## element-ui使用技巧
---

 > el-select获取label值 
``` js
<el-select v-model="value" ref="select" placeholder="请选择"></el-select>
//获取
this.$ref.select.selectLabel
```
> el-table 在flex容器下不会随窗口变化而变化

## vue使用技巧
* 安装tyarn  npm i -g yarn tyarn 

* router-view

Different router the same component vue。 真实的业务场景中，这种情况很多。比如

![图片alt](./img/1.png)

我创建和编辑的页面使用的是同一个 component，默认情况下当这两个页面切换时并不会触发 vue 的 created 或者 mounted 钩子，官方说你可以通过 watch $route 的变化来做处理，但其实说真的还是蛮麻烦的。后来发现其实可以简单的在 router-view 上加上一个唯一的 key，来保证路由切换时都会重新渲染触发钩子了。这样简单的多了。
``` js
<router-view :key="key"></router-view>

computed: {
  key() {
    // 或者 :key="route.fullPath" 只要保证key唯一就可以了
    return this.$route.name !== undefined? this.$route.name + +new Date(): this.$route + +new Date()
  }
 }
 ```





# 性能优化  



# 权限开发流程

前端设计好路由规则,json-tree(里面包含权限)。然后后端根据这个数据规则设计，添加菜单，按钮，tab页的数据。前端登入后，会返回这个权限列表，里面包含按钮，菜单和tab页，还有加载的组件地址。拿到这些数据后，前端先处理路由，将返回的菜单组合成路由，合并本地的一些静态路由。通过addRouter动态挂载到router，这个菜单就能根据返回动态显示了。由于是路由，设计到组件挂载的问题，所有访问的组件地址也是后端去配置的。在动态填在路由时，会出现全部组件一次性加载的问题，所以现将路由都指向同一个组件dashbord，然后通过这个组件在二次指向要访问的组件。组件的按钮权限，都全部挂载到vue.prototype上,然后全判断是否存在权限,才过滤按钮。



# js方面

* 节流:高频事件触发，但在n秒内只会执行一次，所以节流会稀释函数的执行频率 (throttle)
* 防抖:触发高频事件后n秒内函数只会执行一次，如果n秒内高频事件再次被触发，则重新计算时间 (debounce)
[文档概念](https://yuchengkai.cn/docs/frontend/#%E9%98%B2%E6%8A%96)
* 如果我们想获得一个变量的正确类型，可以通过 Object.prototype.toString.call(xx)。这样我们就可以获得类似 [object Type] 的字符串。 
* [浅谈Object.prototype.toString.call()方法](https://www.jianshu.com/p/585926ae62cc)