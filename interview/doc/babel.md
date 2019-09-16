## babel使用
* 官方解释,是下一代JavaScript 语法的编译器。提前使用高级的语法
* 在根目录在配置`babel.config.js`或`.babelrc`文件
* [Babel详情](https://www.babeljs.cn/docs/configuration)
* 使用`babel-preset-env`插件进行语法转义器
* babel-plugin-syntax-dynamic-import 动态import webpack版本4.20.2及以上不需要这个插件了
* babel-plugin-transform-runtime  解决兼容问题,转义es6的新api
* [babel之配置文件.babelrc入门详解](https://juejin.im/post/5a79adeef265da4e93116430)
* [前端工程师需要了解的 Babel 知识](前端工程师需要了解的 Babel 知识)

<details>
<summary>.babelrc配置</summary>

```
{
  "presets": [ // babel-preset-env 语法转义器
    [
      "env",
      {
        "loose": true,
        "modules": false,
        "targets": {
          "browsers": ["> 1%", "last 2 versions", "not ie <= 8"]
        }
      }
    ],
    "stage-2"
  ],
  "plugins": ["transform-vue-jsx"], //插件
  "env": {
    "utils": {
      "presets": [
        [
          "env",
          {
            "loose": true,
            "modules": "commonjs",
            "targets": {
              "browsers": ["> 1%", "last 2 versions", "not ie <= 8"]
            }
          }
        ]
      ],
      "plugins": [
        ["module-resolver", { //模块解析插件
          "root": ["ycloud-ui"],
          "alias": {
            "ycloud-ui/src": "ycloud-ui/lib"
          }
        }]
      ]
    },
    "test": {
      "plugins": ["istanbul"] //instanbul是一个用来测试转码后代码的工具
    }
  }
}

```
</details>

<details>
<summary>syntax-dynamic-import 实现动态import</summary>

```
function nDate() {
  import('moment').then(function(moment) {
    console.log(moment.default().format());
  }).catch(function(err) {
    console.log('Failed to load moment', err);
  });
}
```
</details>

