# MongoDB

## 启动服务

### 指定路径启动

- 使用**dbpath**
- 要在安装目录下的bin目录用管理员打开

```
mongod --dbpath C:\Users\qlhdt\Desktop\MongoDB\test
```

### 开启服务

- **mongo**
- 在另外一个黑窗口打开

## 集合

- 集合类似于一张表  

#### 创建集合

- db.createCollection(name,options);
- - name 必须,是该集合的名字
  - options可选

## 数据库的一些操作

### 查看数据库

```
show.dbs
```

### 切换表

```
use local    切换到local库
```

### 查看集合

```
 show collections
```

## MongoDB的增删改查

#### 增

- 语法

```
db.COLLECTION_NAME.insert(document)


db当前

COLLECTION_NAME   集合

document     bson    其实也就是json(JavaScript)
```

#### 改( update() 方法)

update() 方法用于更新已存在的文档。语法格式如下：

```
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```

- 更新多个

```
db.test_collection.updateMany()
```

- 更新单个

```
db.test_collection.updateOne()
```



参数说明：**

- **query** : update的查询条件，类似sql update查询内where后面的。
- **update** : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
- **upsert** : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
- **multi** : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
- **writeConcern** :可选，抛出异常的级别。

##### 最后一个参数

只更新第一条记录：

```
db.col.update( { "count" : { $gt : 1 } } , { $set : { "test2" : "OK"} } );
```

全部更新：

```
db.col.update( { "count" : { $gt : 3 } } , { $set : { "test2" : "OK"} },false,true );
```

只添加第一条：

```
db.col.update( { "count" : { $gt : 4 } } , { $set : { "test5" : "OK"} },true,false );
```

全部添加加进去:

```
db.col.update( { "count" : { $gt : 5 } } , { $set : { "test5" : "OK"} },true,true );
```

全部更新：

```
db.col.update( { "count" : { $gt : 15 } } , { $inc : { "count" : 1} },false,true );
```

只更新第一条记录：

```
db.col.update( { "count" : { $gt : 10 } } , { $inc : { "count" : 1} },false,false );
```

#### 删

- 精确查找

```
db.collection.remove(
   <query>,
   <justOne>
)




db.ty.remove({"name":"222"});
WriteResult({ "nRemoved" : 1 })
db.ty.find().pretty()
{ "_id" : ObjectId("5b8698f2bd75741cda51a510"), "age" : 18 }
{
        "_id" : ObjectId("5b869e0abd75741cda51a512"),
        "name" : 222,
        "age" : 18,
        "len" : 29,
        "url" : "http://www.baidu.com"
}




也是删除

db.ty.deleteOne({"name":"222"})

db.ty.deleteMany({"name":"222"})   匹配到的全部删除

和remove一样


```

#### 查

- find()
- find().pretty()

```
db.ty.find();返回json


db.ty.find().pretty();返回格式化的json
```

### 条件操作符

- (>) 大于 - $gt
- (<) 小于 - $lt
- (>=) 大于等于 - $gte
- (<= ) 小于等于 - $lte
- **其实也就是筛选**

```
db.ty.find({"likes" : {$gt : 100}})
```

