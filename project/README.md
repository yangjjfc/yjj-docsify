# 地址
 [前端技术管理交流](https://www.yuque.com/iscott/tl)

 ## 代码规范
 *  husky + lintstaged + eslint 基本上能杜绝大部分代码问题，再加上commitlint规范下git提交信息
 * 添加相关依赖 yarn add husky lint-staged @commitlint/config-conventional @commitlint/cli -D
 * 添加配置文件`commitlint.config.js`到更目录中
 * `package`中添加相关规则
 * [husky](https://zhuanlan.zhihu.com/p/35913229) 
 * [lint-staged](https://segmentfault.com/a/1190000009546913) 
 * [Commitlint](https://segmentfault.com/a/1190000017790694)

<details>
<summary>commitlint.config.js代码</summary>

 ```
module.exports = {
    extends: [
        '@commitlint/config-conventional'
    ],
    rules: {
        'type-enum': [2, 'always', [
            'upd', 'feat', 'fix', 'refactor', 'docs', 'chore', 'style', 'revert'
        ]],
        'type-case': [0],
        'type-empty': [0],
        'scope-empty': [0],
        'scope-case': [0],
        'subject-full-stop': [0, 'never'],
        'subject-case': [0, 'never'],
        'header-max-length': [0, 'always', 72]
    }
};
 ```
</details>

<details>
<summary>package.json代码</summary>

 ```
"husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "commit-msg": "commitlint -e $HUSKY_GIT_PARAMS"
    }
  },
  "lint-staged": {
    "src/**/*.js": ["yarn lint", "git add"]
  }
 ```
</details>

### 添加提交lint规则
* upd：更新某功能（不是 feat, 不是 fix）
* feat：新功能（feature）
* fix：修补bug
* docs：文档（documentation）
* style： 格式（不影响代码运行的变动）
* refactor：重构（即不是新增功能，也不是修改bug的代码变动）
* test：增加测试
* chore：构建过程或辅助工具的变动
```
git commit -m "upd：新增发货功能" //:后面有个空格
```



[阿里大佬浅谈大型项目前端架构设计]( https://juejin.im/post/5cea1f705188250640005472)













 ##TODO
1. 写了个通用脚手架，避免项目模版搭建
2. 写了不少脚本，帮助内部童靴校验， 加速运维相关的操作
3. 写了一些内部chrome插件， 打通内部杂七杂八的平台连通性，比如一键生成日报，周报，项目进度
4. 还有一些监控，报警发邮件， 谁的页面出错自己去check
5. pptr自动测试等等







