## postcss
* postcss是用来处理css的一个工具，在它基础上还有这种插件可以配合他一起使用
* 先把css代码解析抽象成AST，然后在交由配合的插件处理，一般是加上浏览器前缀，兼容之类的
* 在webpack中使用，利用`postcss-loader`插件配合`autoperfixer`使用，在更目录下创建postcss.config.js
* [postcss-css](https://www.ibm.com/developerworks/cn/web/1604-postcss-css/index.html)
* [关于PostCSS的一点小科普](https://juejin.im/post/5a31ef8f6fb9a0450b6666a0)

## css
* 生产环境引入`mini-css-extract-plugin`,作用抽离css，每个页面1个css
* `optimize-css-assets-webpack-plugin`,压缩css
* [css打包压缩](https://blog.csdn.net/lsvtogergo/article/details/84959009)
* [css打包压缩](https://zhuanlan.zhihu.com/p/37251575)