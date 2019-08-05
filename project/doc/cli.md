## 前端cli
 * 利用sheel脚本执行命令,开始先输入项目名称
 * 然后选择需要安装的版本,例如`vue-system`管理系统,`vue-mobile`移动端
 * 根据选择的版本执行相应的子sheel
 * 执行相应的原生脚手架安装,比如选择vue,就会去执行vue create app操作
 * 执行相应的node脚本,对生成想项目进行`del`,`copy`,`inject`等一系列操作,主要看配置文件中的配置`configuration.js`


## 使用
* 可以通过 npm i ycloud-cli -g全局安装
* 因为使用的是sheel,所以执行命令时windows操作系统需要在git bash下执行
* 在需要创建的文件目录下,执行ycloud,然后按提示输入就行
* vue create app的时候需要去自行选择,eslint,vue-router,vuex,scss(node-scss)这种必须要选择