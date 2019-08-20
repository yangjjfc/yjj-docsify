<!--
 * @Author: yangjj
 * @Date: 2019-08-13 09:01:16
 * @LastEditors: yangjj
 * @LastEditTime: 2019-08-13 19:16:09
 * @Description: file content
 -->
## postcss

- postcss 是用来处理 css 的一个工具，在它基础上还有这种插件可以配合他一起使用
- 先把 css 代码解析抽象成 AST，然后在交由配合的插件处理，一般是加上浏览器前缀，兼容之类的
- 在 webpack 中使用，利用`postcss-loader`插件配合`autoperfixer`使用，在更目录下创建 postcss.config.js
- [postcss-css](https://www.ibm.com/developerworks/cn/web/1604-postcss-css/index.html)
- [关于 PostCSS 的一点小科普](https://juejin.im/post/5a31ef8f6fb9a0450b6666a0)

## css

- 生产环境引入`mini-css-extract-plugin`,作用抽离 css，每个页面 1 个 css
- `optimize-css-assets-webpack-plugin`,压缩 css
- [css 打包压缩](https://blog.csdn.net/lsvtogergo/article/details/84959009)
- [css 打包压缩](https://zhuanlan.zhihu.com/p/37251575)

## babel-plugin-component

- 按需加载
-

```
import {button} from 'element'
就等于
var button = require('components/lib/button')
require('components/lib/button/style.css')
```

- [element 按需加载设置](https://segmentfault.com/a/1190000015884948)

## 打包优化

- optimization 下的 splitChunks==>cacheGroups
- runtimeChunk 配置,会多生产一个 runtime 文件,作用是 2 次编译时,只有改变的文件,hash 值才会改变,有利于缓存
- tree shaking 只支持 es module 没用的代码不打包
- [PWA](https://blog.csdn.net/i10630226/article/details/78885664)
- [workbox-webpack-plugin](https://blog.csdn.net/mjzhang1993/article/details/79584854)

```
module.exports = {
  //...
  optimization: {
    usedExports: true,//生产环境默认打开的
    runtimeChunk:{
        name:'runtime'
    },
    splitChunks: {
      chunks: 'async', //同步/异步
      minSize: 30000, //基于多少时才做代码分割
      maxSize: 0,
      minChunks: 1,
      maxAsyncRequests: 5,
      maxInitialRequests: 3,
      automaticNameDelimiter: '~',
      name: true,
      cacheGroups: { //分组打包,针对同步打包
        vendors: {
          test: /[\\/]node_modules[\\/]/,
          priority: -10
        },
        default: {
          minChunks: 2,
          priority: -20,
          reuseExistingChunk: true //以打包的就不会重复打包了
        }
      }
    }
  }
};
```

## webpack-node-externals
* 
```
var nodeExternals = require('webpack-node-externals');
...
module.exports = {
    ...
    target: 'node', // in order to ignore built-in modules like path, fs, etc.
    externals: [nodeExternals()], // in order to ignore all modules in node_modules folder
    ...
};
```
* [webpack-node-externals](https://www.npmjs.com/package/webpack-node-externals)

## Tree-Shaking
* webpack4+babel7才能过滤node包中的tree-sharking功能;
* [你的Tree-Shaking并没什么卵用](https://juejin.im/post/5a5652d8f265da3e497ff3de)
* [Webpack 4 教程 - 7. 通过“tree shaking”减少打包的尺寸](https://segmentfault.com/a/1190000016767989)

## output配置
```
output: {
    path: path.resolve(process.cwd(), './lib'),
    publicPath: '/dist/',
    filename: 'ycloud-ui.common.js',
    chunkFilename: '[id].js',
    // libraryExport: 'default', // var MyDefaultModule = _entry_return_.default
    library: 'YCLOUD', // 注入全局变量
    libraryTarget: 'commonjs2'// 入口起点的返回值将分配给 module.exports 对象
  }
```


## 自动注册组件