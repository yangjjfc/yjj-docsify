<!--
 * @Author: yangjj
 * @Date: 2019-08-13 09:01:16
 * @LastEditors: yangjj
 * @LastEditTime: 2019-08-13 19:04:31
 * @Description: file content
 -->
## 自我介绍
> 你好,面试官。我做前端这行差不多有5年的时间了，在这5年的期间里，开发过各种项目，有移动端的h5页面，pc端的管理系统,商城,还有小程序。在当前呆的这家公司，主要的职责除了普通的业务模块开发,就是做一些规范，技术选型，ui组件库开发维护这些工作。个人呢，前端也是比较感兴趣的，会时常是看下别人发表的文章，学习他们的方法，经验。也会时常也一些demo,可行的话会用于生产中

## 前端规范
* 一定的命名规范,比如取名必须语义化,css命名是要用中-拼接,js名称采用驼峰方式,组件使用时,使用的组件首字母必须大写
* 代码中采用了eslint,自动校验语法标准,利用vscode,实现保存自定格式化
* 利用husky+lintstaged+commitlint,校验提交规范,提交前会执行代码的eslint检测
* 文件放置目录的规范,比如公共的mixin函数,需要放在utils下的mixin文件夹下等等
* 所有的开发都不能再主分支开发,一般只有前端主管才有主分支权限


## 难点举例
需求
1. 在开发erp项目时遇到一个表格table的复杂需求,
2. 用户可以去自定义设计表格列的顺序,
3. 可以去改变表格heade的名称,
4. 还能对表格相关的列进行锁列,不随滚动条滚动
5. 自动计算表格的高度,不能让表格超出屏幕高度,
6. 操作按钮数量也有限制,超过4个就把第四个显示成更多,多余的按钮都在更多里面,小于等于四个就正常显示,
7. 可以隐藏相关列

解决
1. 表格还是使用element-ui的table开发,本身就支持锁列的操作,继承属性是v-bind="$attrs"和v-on="$listeners"
2. 想到数据是list,那我们的列也可以使用list数组的形式,通过vue的forin是循环遍历出列
3. 这里就定义了一个props为config,array形式JSON list
4. json: type: [index,operate],solt,filed会以v-html的形式去渲染,hide是否显示这行v-if


## 性能优化
1. 使用图片整合技术,减少http请求
2. 使用iconfont,svg代替小图标,在打包时,小图片开启base64位压缩
3. 打包时,利用webpack插件,打包成gzip,然后ngnix配置也得开启gzip,组件模块化打包。import懒加载相关组件。
4. 使用cnd加载js的插件,减少三方依赖包的大小。开发时可利用webpack中`externals`配置,import相关插件。
5. 使用预加载技术，当空闲时间，去加载下个页面需要用到的组件，`<link rel="prefetch">`
6. 延迟加载，一些展示页，当滚动条滚动到相关位置时，才去加载相关图片
7. 添加 Expires 或 Cache-Control 响应头,强制缓存策略。
8. 减少 DNS 查询,查询把资源分布到 2 个域名上（最多不超过 4 个）
9. 减少dom的操作 `document.getElementsByTagName('*').length`; 可以计算出标签的数量
* [前端性能优化](https://csspod.com/frontend-performance-best-practices/)




 ## http
 http2 优点
 * 多路复用，降低多个请求的开销
 * 客户端与服务端只需要建立一个TCP链接，他能保持持久化，以便复用

## js类型

* js类型(7种): string,number,boolean,null,undefine,object,symbol


## 深拷贝
[Javascript之深拷贝](https://zhuanlan.zhihu.com/p/23251162)
