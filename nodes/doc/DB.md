## 数据库分类
* 关系型数据库(RDBMS)
 - Mysql,Qracle,Db2
* 非关系型数据库(NO SQL)
 - MongoDB,Redis

## MongoDB
* 简单、灵活，类似于Json，可自由添加
* 偶数版是稳定版
* 安装完后再.bin文件夹下创建`mongod.cfg`配置文件,还需要设置环境变量
* 然后执行`mongod -f mongod.conf`启动服务
* 执行 `mongo` 连接服务
* mongod --dbpath '指定数据库文件地址'  --port 端口号
* 参考文献
  - [下载地址](http://dl.mongodb.org/dl/win32/x86_64)
  - [安装](https://www.cnblogs.com/damon-/p/9144320.html)
  - [MongoDB之conf配置文件详解(五)](https://www.cnblogs.com/cwp-bg/p/9479945.html)
  - [文档地址](https://docs.mongodb.com)
  - [手册](http://www.shouce.ren/api/view/a/6191)

* D:\Program Files\mongodb\bin>mongod -dbpath "D:\Program Files\mongodb\data\db" 启动mongodb服务
```js
show dbs; //显示全部数据库
db.dropDatabase() //删除数据库
db.zh_tops.drop() //删除表
use test //切换数据库,没有则新增数据库test
db.test.insert({name:'yangjj'}) //插入一条数据

db.createCollection('test'); //创建 集合
show collections //查看已有集合

//插入
db.test.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
//查看已插入文档
db.test.find();
db.test.find().pretty()
//还有一个 findOne() 方法

//更新
db.test.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}});
//如果你要修改多条相同的文档,则需要设置 multi 参数为 true
db.test.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}},{multi:true})


//删除
db.test.remove({'title':'MongoDB 教程'})
//如果你想删除所有数据
db.test.remove({})


db.test.getIndexes()

db.test.totalIndexSize()

```