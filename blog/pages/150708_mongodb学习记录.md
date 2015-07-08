[GitHubBlog](https://github.com/bbxytl/bbxytl.github.com/tree/master/blog#home--githubblog) /
=====
# [mongodb 学习记录](https://github.com/bbxytl/bbxytl.github.com/blob/master/blog/pages/150708_mongodb学习记录.md#githubblog-)
---

### 初始化
---

- 为使用mongodb数据库，先建立一个用于存储数据库的工作目录：(目录可以建立在任意位置；在使用虚拟机时，不要建立在共享文件夹里！会报 100 号错误。)
```shell
mkdir db_simple
```

- 接下来创建要使用的目录：
```shell
cd db_simple
mkdir bin
mkdir data
mkdir conf
mkdir log
cd conf
vim mongod.conf
# 插入如下内容：
port = 10002    # 使用的端口
dbpath = data   # 存放数据库文件的目录
dblog = log/mongod.log  # 指定日志文件
fork = true     # linux 下生效
# 保存后退出
cd ..
cp <mongodb/bin-path>/mongod bin/  # 为方便将 mongd copy 到当前目录的bin下
```

### 启动服务
---

```shell
./bin/mongod -f conf/mongod.conf
```

### 数据库连接与退出
---

- 数据库连接
```shell
cp <mongodb/bin-path>/mongo bin/
./bin/mongo 127.0.0.1:10002/test
```

- 数据库退出
```shell
> use admin
> db.shutdownServer()
```

### 数据库基本操作
---

- 创建 数据库
```shell
use imooc
```

- 插入与查询
```shell
db.imooc_collection.insert({x:1})   # 插入一条数据
show dbs                    # 显示所有数据库，一个数据库没有数据时，show不出来
db.imooc_collection.find()  # 查看所有数据
# 一条数据有一个 "_id", 不能重复！
db.imooc_collection.find().count() # 对查询的数据计数
# 对查询结果跳过前 3 行，限制之输出两行，以key=x进行排序，排序方式（1:正序,-1:倒序）
db.imooc_collection.find().spikp(3).limit(2).sort({x:1})
```

- 更新
```shell
# 将 x 为 1 更新为 999
db.imooc_collection.update({x:1},{x:999})
db.imooc_collection.insert({x:100, y:100, z:100})
# 要更新 y 为 99 ， 不能使用上面的方法，因为这是一条数据，相当于一个词典，会直接替换掉，
# 要更新部分内容，使用 $set 操作符，内容存在的会被更新，不存在的保持原样，如下面的方式：
db.imooc_collection.update({z:100}, {$set:{y:99}})
db.imooc_collection.find({z:100}) # 此时已正常更新
```

- 更新不存在的数据
```shell
# 使用 update的第三个参数来指定是否要在查找失败的情况下插入数据
db.imooc_collection.update({c:1}, {c:2}, true)
```

- 更新多条数据
```shell
# 默认情况下如果有多条数据存在，只会更新第一条，其他的不动
# 如果想全部更新，需要使用 $set 操作符
db.imooc_collection.update({c:1},{$set:{c:2}},false, true)
```

- 删除操作
```shell
# 默认情况下 删除操作不允许不传参数，同时删除操作默认删除所有查找到的数据
db.imooc_collection.remove({c:2})
# 对于某张表使用删除： drop()
db.imooc_collection.drop()
```

### 索引
---

- 查看索引
```shell
db.imooc_collection.getIndexes()
```

- 创建索引
```shell
# 在数据文档比较多的情况下，不能使用这个命令，应在创建数据库时就创建好索引，
# 否则会严重影响数据库性能
db.imooc_collection.ensureIndex({x:1})
```

- 创建一个过期索引
```shell
# 使用如下数据，expireAfterSeconds 指定过期时间
db.imooc_collection.ensureIndex({time:1}, {expireAfterSeconds:30})
```
> 1. 存储在过期索引字段的值必须是指定的时间类型：必须是ISODate或者ISODate数组，不能使用时间戳，否则不能被自动删除；
> 2. 如果指定了ISODate数组，则按照最小的时间进行删除；
> 3. 过期索引不能是复合索引；
> 4. 删除时间不能是不精确的；删除过程是由后台程序每 60s 跑一次，而且删除也需要一些时间，所以存在误差。

- 创建全文索引
```shell
# 和之前的创建索引相同，但是 value 使用 "text" 来表示在字段 key 上创建一个全文索引
db.imooc_collection.ensureIndex({key:"text"})
db.imooc_collection.ensureIndex({key1:"text",key2:"text"})
# 表示对集合中所有字段创建一个大的全文索引
db.imooc_collection.ensureIndex({"$**":"text"})
```

- 使用全文索引进行查询
```shell
db.imooc_collection.find({$text:{$search:"coffee"}})
db.imooc_collection.find({$text:{$search:"aa bb cc"}})
db.imooc_collection.find({$text:{$search:"aa bb -dd"}}) # 加 - 号说明不包含 dd
db.imooc_collection.find({$text:{$search:"\"aa\" bb cc"}})
```

- 全文索引的相似度
`$meta`操作符: `{socre: {$meta:"textScore"}}`
写在查询条件后面可以返回返回结果的相似度，与 sort 一起使用，可以达到很好的实用效果。
```shell
db.imooc_collection.find({$text:{$search:"aa bb"}}, {socre:{$meta:"textScore"}})
# 可以根据相似度对返回结果进行排序
db.imooc_collection.find({$text:{$search:"aa bb"}}, {socre:{$meta:"textScore"}}).sort({socre:{$meta:"textScore"}})
```

- 全文索引的使用限制
    - 每次查询只能指定一个 `$text` 查询
    - `$text` 查询不能出现在 `$nor` 查询中
    - 查询中如果包含了 `$text` ，hint 不在起作用
    - MongoDB全文索引还不支持中文



## **附录**
- **[博客园-Blog](http://www.cnblogs.com/lomper/)**
- **[GitHub-Blog](http://bbxytl.github.io/)**
- **关注微信订阅号：LomperWay**：     
    ![关注微信订阅号](./images/qrcodes/qrcode_100.jpg)

