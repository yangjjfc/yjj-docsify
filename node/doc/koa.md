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

* `koa-static` 处理静态文件
 - [koa2使用koa-static静态文件](https://www.jianshu.com/p/188b0835b2e0)

* `koa2-cors` 跨域请求实现
 - [node.js 应答跨域请求实现 （以koa2-cors为例）](https://www.jianshu.com/p/5b3acded5182)

* `koa-router` 请求处理
 - [koa-router使用指南](https://blog.csdn.net/luchuanqi67/article/details/81475077)