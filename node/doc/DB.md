## 数据库分类
* 关系型数据库(RDBMS)
 - Mysql,Qracle,Db2
* 非关系型数据库(NO SQL)
 - MongoDB,Redis

## MongoDB
* 简单、灵活，类似于Json，可自由添加
* 偶数版是稳定版
[安装](https://www.cnblogs.com/damon-/p/9144320.html)
* 设置环境变量
mongod --dbpath "D:\Program Files (x86)\mongodb\data\db"
* mongod --dbpath '指定数据库文件地址'  --port 端口号
* 启动mongo
[下载地址](http://dl.mongodb.org/dl/win32/x86_64)
[文档地址](https://docs.mongodb.com)

数据库
 - 数据库的服务器 
  - 保存数据
  - mongod启动服务端
 - 数据库的客户端
  - 操作数据
  - mongo启动客户端

添加根目录mongod.cfg
