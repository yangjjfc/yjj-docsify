<!--
 * @Author: yangjj
 * @Date: 2019-08-13 19:16:36
 * @LastEditors: yangjj
 * @LastEditTime: 2019-08-13 19:22:34
 * @Description: file content
 -->
## process
* [process.argv与命令行工具](https://juejin.im/post/5a976e87f265da4e8c453eec)

<details>
<summary>从代码中看出process.argv[2]是参数的起始点</summary>

```js
process.argv
>>>命令行中输入
node p1.js --a -b c
<<< 输出
E:\w>node p1.js --a -b c
[ 'C:\\Program Files\\nodejs\\node.exe',
'E:\\w\\p1.js',
'--a',
'-b',
'c' ]
```
</details>


## chokidar
* 有文件变化时只对该文件进行操作
[Nodejs文件监控chokidar](https://www.cnblogs.com/cool-fire/p/6565242.html)

<details>
<summary>chokidar</summary>

```js
const path = require('path');
// process.cwd() 方法返回 Node.js 进程的当前工作目录。
const templates = path.resolve(process.cwd(), './examples/pages/template');
// Nodejs文件监控chokidar
const chokidar = require('chokidar');
let watcher = chokidar.watch([templates]);
// 监听
watcher.on('ready', function () {
  watcher
    .on('change', function () {
      exec('npm run i18n');
    });
});

function exec (cmd) {
  return require('child_process').execSync(cmd).toString().trim();
}

```

</details>