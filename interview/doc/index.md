<!--
 * @Author: yangjj
 * @Date: 2019-08-13 09:01:16
 * @LastEditors: yangjj
 * @LastEditTime: 2019-08-20 15:58:13
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
 * 在 HTTP/1 中，每次请求都会建立一次TCP连接，也就是我们常说的3次握手4次挥手，这在一次请求过程中占用了相当长的时间，即使开启了 Keep-Alive ，解决了多次连接的问题，但是依然有两个效率上的问题：

 * 第一个：串行的文件传输。当请求a文件时，b文件只能等待，等待a连接到服务器、服务器处理文件、服务器返回文件，这三个步骤。我们假设这三步用时都是1秒，那么a文件用时为3秒，b文件传输完成用时为6秒，依此类推。（注：此项计算有一个前提条件，就是浏览器和服务器是单通道传输）
 * 第二个：连接数过多。我们假设Apache设置了最大并发数为300，因为浏览器限制，浏览器发起的最大请求数为6（Chrome），也就是服务器能承载的最高并发为50，当第51个人访问时，就需要等待前面某个请求处理完成。

 * HTTP2采用二进制格式传输，取代了HTTP1.x的文本格式，二进制格式解析更高效。
 * 多路复用代替了HTTP1.x的序列和阻塞机制，所有的相同域名请求都通过同一个TCP连接并发完成。在HTTP1.x中，并发多个请求需要多个TCP连接，浏览器为了控制资源会有6-8个TCP连接都限制。
 * HTTP2中 同域名下所有通信都在单个连接上完成，消除了因多个 TCP 连接而带来的延时和内存消耗。 单个连接上可以并行交错的请求和响应，之间互不干扰

##缓存
 * 前端缓存分为强缓存和协商缓存
 * `Cache-Control/Expires`,浏览器会判断缓存是否过期,未过期就直接使用强缓存，Cache-Control的 max-age 优先级高于 Expires
 * 当缓存已经过期时，使用协商缓存
 * 唯一标识方案:  Etag(response 携带) & If-None-Match(request携带，上一次返回的 Etag): 服务器判断资源是否被修改
 * Etag是一串hash值,字节长度+修改时间
 * 最后一次修改时间: Last-Modified(response) & If-Modified-Since (request，上一次返回的Last-Modified)
 * 判断是否一直,一直就返回304通知浏览器使用缓存
 * 不一致则返回新资源
 * Etag 的优先级高于 Last-Modified
 * Last-Modified 缺点: 
   - 周期性修改，但内容未变时，会导致缓存失效
   - 最小粒度只到 s， s 以内的改动无法检测到

## 3次握手
* 客户端发送 syn(同步序列编号) 请求，进入 syn_send 状态，等待确认
* 服务端接收并确认 syn 包后发送 syn+ack 包，进入 syn_recv 状态
* 客户端接收 syn+ack 包后，发送 ack 包，双方进入 established 状态




## js类型

* js类型(7种): string,number,boolean,null,undefine,object,symbol
* typeof能区分的五种基本类型：string、boolean、number、undefined、symbol和函数function
* typeof只能区分值类型，对引用类型无能为力，只能区分函数function
* instanceof：用于判断引用类型属于哪个构造函数的方法
* 判断一个变量是否为数组 Object.prototype.toString.call(arr) === "[object Array]" arr instanceOf Array

## 深拷贝
[Javascript之深拷贝](https://zhuanlan.zhihu.com/p/23251162)


## 浏览器输入URL回车
* URL解析,判断是`http,https,ftp`等
* DNS域名解析
 - 先到各种缓存信息中查找
 - DNS服务器查找
* 建立3次握手`TCP`
 - 客户端向服务器发送syn包,请求连接
 - 服务器接收到syn包后,会返回syc+ack包,确认信息
 - 客户端收到返回的包后,返回ackb包,建立握手
* 发送HTTP请求
* 服务器处理请求,返回相应的请求内容
* 浏览器解析返回的数据
 - 解析html,生产dom树
 - 解析css,生产css规则树
* 浏览器布局渲染

## Array处理
[关于Array的特别处理](https://juejin.im/post/5d579cd36fb9a06aea6190db)

