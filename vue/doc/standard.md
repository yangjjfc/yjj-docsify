 ## 代码规范
 * 利用eslint做代码规范,vscode配置保存后自动格式化
 * husky + lintstaged + eslint 基本上能杜绝大部分代码问题，再加上commitlint规范下git提交信息
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


