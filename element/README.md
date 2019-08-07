目录结构
```
├── build //打包命令
│   ├── bin 
│   │   ├── build-entry.js //生成组件index.js文件
│   │   ├── build-locale.js //国际化语言打包----暂不去了解
│   │   ├── gen-cssfile.js //生成packages/theme-chalk下的scss文件
│   │   ├── gen-indices.js //解析doc的md文件,生成algoliasearch的搜索
│   │   ├── i18n.js //生成api官网不同语言,
│   │   ├── iconInit.js //生成icon列表
│   │   ├── new.js //创建一个新的组件
│   │   ├── new-lang.js //创建新的语言
│   │   ├── template.js //在线
│   │   └── version.js //产出项目版本
│   ├── config.js //打包的一些工具方法
│   ├── deploy-ci.sh
│   ├── deploy-faas.sh
│   ├── gen-single-config.js //产出一些先关配置
│   ├── git-release.sh
│   ├── md-loader //md相关解析
│   │   ├── config.js
│   │   ├── containers.js
│   │   ├── fence.js
│   │   ├── index.js
│   │   └── util.js
│   ├── release.sh
│   ├── webpack.common.js //common打包,全部组件都打包成一个js
│   ├── webpack.component.js //组件单独打包
│   ├── webpack.conf.js //umd打包,生成index文件
│   ├── webpack.demo.js //演示打包
│   └── webpack.test.js //测试打包
├── CHANGELOG.zh-CN.md
├── components.json  //全部组件目录
├── element_logo.svg //logo
├── examples //例子,可在线运行
│   ├── app.vue
│   ├── assets
│   ├── bus.js
│   ├── color.js
│   ├── components
│   ├── demo-styles
│   ├── docs
│   ├── dom
│   ├── entry.js //dev入口文件
│   ├── favicon.ico
│   ├── i18n 
│   ├── icon.json
│   ├── index.tpl //模板
│   ├── nav.config.json //侧边导航
│   ├── pages //显示的内容
│   ├── play //单个组件测试
│   ├── play.js
│   ├── route.config.js //路由配置
│   ├── util.js
│   └── versions.json //记录先关版本
├── FAQ.md 
├── LICENSE
├── Makefile //make 命令
├── package.json
├── packages //组件目录
│   ├── dialog
│   ├── file-upload
│   ├── icon
│   ├── pagination
│   ├── region-picker
│   ├── table-tree
│   └── theme-chalk
├── README.md
├── src  //工具文件
│   ├── directives
│   ├── index.js
│   ├── locale
│   ├── mixins
│   ├── transitions
│   └── utils
├── test //测试文件
│   └── unit
├── types //先关ts
│   ├── component.d.ts
│   ├── dialog.d.ts
│   ├── file-upload.d.ts
│   ├── icon.d.ts
│   ├── pagination.d.ts
│   ├── region-picker.d.ts
│   └── table-tree.d.ts
└── yarn.lock
```