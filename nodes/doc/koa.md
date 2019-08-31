<!--
 * @Author: yangjj
 * @Date: 2019-08-21 09:34:21
 * @LastEditors: yangjj
 * @LastEditTime: 2019-08-21 14:43:25
 * @Description: file content
 -->
## koa理解
[koa2从搭建项目到实现API](https://segmentfault.com/a/1190000017022073)
[基于Node的Koa2项目从创建到打包到云服务器指南](https://segmentfault.com/a/1190000011097690)

## koa插件
* `koa-bodyparser` 处理post提交数据,直接能获取结果,读取`ctx.request.body`
 - [koa post提交数据 koa-bodyparser中间件](https://www.jianshu.com/p/93032efe97d0)

* `koa-multer` 处理文件上传,但和`koa-route`有冲突,所以建议使用`koa-router`
 - [使用koa-multer实现文件上传并自定义文件名和目录](http://www.ptbird.cn/koa-multer-file-upload.html)

* `koa-body`可代替`koa-bodyparser`和`koa-multer`
 - [koa2 使用 koa-body 代替 koa-bodyparser 和 koa-multer](http://www.ptbird.cn/koa-body.html)

* `koa-static` 处理静态文件,使文件可被外部访问
 - [koa2使用koa-static静态文件](https://www.jianshu.com/p/188b0835b2e0)

* `koa2-cors` 跨域请求实现
 - [node.js 应答跨域请求实现 （以koa2-cors为例）](https://www.jianshu.com/p/5b3acded5182)

* `koa-router` 请求处理
 - [koa-router使用指南](https://blog.csdn.net/luchuanqi67/article/details/81475077)

* `koa-views` 模板中间件需要和模板引擎结合`ejs`
 - [中间件koa-views](https://www.jianshu.com/p/33e55974a32f)

* `ejs`   JavaScript 模板引擎
 - [ejs](https://ejs.bootcss.com/)

* `koa-logger` 日志
 - [](https://mobilesite.github.io/2017/04/29/develop-backend-service-with-koa2/)
* `koa-json` 美观的输出JSON 
* `koa-onerror` 错误处理中间件
* `cross-env` 跨平台配置环境变量的模块
* `debug` 理解为一个封装过的console.log()
* `nodemon` 修改文件可以自动重启服务
* `log4js` 本地输出log



* `SuperAgent` [客户端请求代理模块](https://www.jianshu.com/p/1432e0f29abd)
* `cheerio` [是jquery核心功能的一个快速灵活而又简洁的实现，主要是为了用在服务器端需要对DOM进行操作的地方](https://www.jianshu.com/p/629a81b4e013)
* `puppeteer` [打开网页并截图](https://www.jianshu.com/p/56babda610f9)
* `debug` 理解为一个封装过的console.log()

## 生成koa项目
* 安装`npm install -g koa-generator`
* 执行`koa2 test_koa`