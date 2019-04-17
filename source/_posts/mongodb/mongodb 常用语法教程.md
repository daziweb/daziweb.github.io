---
title: mongodb 常用语法教程
tags:
  - mongodb
categories:
  - mongodb
comments: true
---

## 创建数据库

MongoDB use DATABASE_NAME 用于创建数据库。该命令如果数据库不存在，将创建一个新的数据库， 否则将返回现有的数据库。

##### 语法

use DATABASE语句的基本语法如下：

``` javascript
use DATABASE_NAME
```

##### 例子：

如果想创建一个数据库名称为 **\<mydb>**， 那么 **use DATABASE** 语句应该如下：

``` javascript
>use mydb
switched to db mydb
```

要检查当前选择的数据库使用命令 **db**


``` javascript
>db
mydb
```

如果想查询数据库列表，那么使用命令 **show dbs**.


``` javascript
>show dbs
local     0.78125GB
test      0.23012GB
```

所创建的数据库（mydb）不存在于列表中。要显示的数据库，需要至少插入一个文档进去。

``` javascript
>db.movie.insert({"name":"yiibai tutorials"})
>show dbs
local      0.78125GB
mydb       0.23012GB
test       0.23012GB
```

**MongoDB的默认数据库是test。 如果没有创建任何数据库，那么集合将被保存在测试数据库。**

<!-- more -->

## 删除数据库

MongoDB db.dropDatabase() 命令用于删除现有的数据库。

##### 语法

dropDatabase()指令的基本语法如下：

``` javascript
db.dropDatabase()
```

这将删除选定的数据库。如果没有选择任何数据库，那么它会删除默认的“test”数据库

##### 例子：

如果想删除新的数据库 **\<mydb>**, 那么 **dropDatabase()** 命令将如下所示：

``` javascript
>use mydb
switched to db mydb
>db.dropDatabase()
>{ "dropped" : "mydb", "ok" : 1 }
>
```

## 创建集合

MongoDB 的 db.createCollection(name, options) 用于创建集合。 在命令中, name 是要创建集合的名称。 Options 是一个文档，用于指定集合的配置

|  参数  |  类型  |  描述  |
| --- | --- | --- |
|  Name   |  String   |  要创建的集合的名称   |
|  OPtions    |  Document   |  （可选）指定有关内存大小和索引选项   |

选项参数是可选的，所以需要指定集合的唯一名字。

##### 语法

createCollection()方法的基本语法如下

``` javascript
>use test
switched to db test
>db.createCollection("mycollection")
{ "ok" : 1 }
>
```

可以通过使用 **show collections** 命令来检查创建的集合

``` javascript
>show collections
mycollection
system.indexes
```

##### 选项列表

|   字段  |   类型  |  描述   |
| --- | --- | --- |
|   capped  |   Boolean  |   （可选）如果为true，它启用上限集合。上限集合是一个固定大小的集合，当它达到其最大尺寸会自动覆盖最老的条目。 如果指定true，则还需要指定参数的大小。  |
|   autoIndexID  |   Boolean  |  （可选）如果为true，自动创建索引_id字段。默认的值是 false.   |
|   size  |   number  |  （可选）指定的上限集合字节的最大尺寸。如果capped 是true，那么还需要指定这个字段。   |
|  max   |   number  |   （可选）指定上限集合允许的最大文件数。  |

尽管插入文档，MongoDB首先检查字段集合的上限大小，那么它会检查最大字段。

##### 语法:

``` javascript
>db.createCollection("mycol", { capped : true, autoIndexID : true, size : 6142800, max : 10000 } )
{ "ok" : 1 }
>
```

在MongoDB中并不需要创建集合。 当插入一些文档 MongoDB 会自动创建集合。

``` javascript
>db.yiibai.insert({"name" : "yiibai"})
>show collections
mycol
mycollection
system.indexes
yiibai
>
```

## 删除集合

MongoDB 的 db.collection.drop() 用于从数据库中删除集合。

##### 语法

drop() 命令的基本语法如下

``` javascript
db.COLLECTION_NAME.drop()
``` 

##### 例子

下面给出的例子将删除给定名称的集合：**mycollection**

``` javascript
>use mydb
switched to db mydb
>db.mycollection.drop()
true
>
```

## 插入文档

将数据插入到MongoDB集合，需要使用MongoDB 的 insert() 方法。

##### 语法

insert()命令的基本语法如下：

``` javascript
>db.COLLECTION_NAME.insert(document)
```

##### 例子

``` javascript
>db.mycol.insert({
   _id: ObjectId(7df78ad8902c),
   title: 'MongoDB Overview', 
   description: 'MongoDB is no sql database',
   by: 'yiibai tutorials',
   url: 'http://www.yiibai.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 100
})
```

这里 mycol 是我们的集合名称，它是在之前的教程中创建。如果集合不存在于数据库中，那么MongoDB创建此集合，然后插入文档进去。

在如果我们不指定_id参数插入的文档，那么 MongoDB 将为文档分配一个唯一的ObjectId。

_id 是12个字节十六进制数在一个集合的每个文档是唯一的。 12个字节被划分如下：

``` javascript
_id: ObjectId(4 bytes timestamp, 3 bytes machine id, 2 bytes process id, 3 bytes incrementer)
```

要以单个查询插入多个文档，可以通过文档 insert() 命令的数组方式。

##### 例子

``` javascript
>db.post.insert([
{
   title: 'MongoDB Overview', 
   description: 'MongoDB is no sql database',
   by: 'yiibai tutorials',
   url: 'http://www.yiibai.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 100
},
{
   title: 'NoSQL Database', 
   description: 'NoSQL database doesn't have tables',
   by: 'yiibai tutorials',
   url: 'http://www.yiibai.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 20, 
   comments: [	
      {
         user:'user1',
         message: 'My first comment',
         dateCreated: new Date(2013,11,10,2,35),
         like: 0 
      }
   ]
}
])
```

## 查询文档

要从集合查询MongoDB数据，需要使用MongoDB的 find()方法。

##### 语法

find()方法的基本语法如下

``` javascript
>db.COLLECTION_NAME.find()
```

find() 方法将在非结构化的方式显示所有的文件。 如果显示结果是格式化的，那么可以用pretty() 方法。

##### 语法

``` javascript
>db.mycol.find().pretty()
```

##### 例子

``` javascript
>db.mycol.find().pretty()
{
   "_id": ObjectId(7df78ad8902c),
   "title": "MongoDB Overview", 
   "description": "MongoDB is no sql database",
   "by": "yiibai tutorials",
   "url": "http://www.yiibai.com",
   "tags": ["mongodb", "database", "NoSQL"],
   "likes": "100"
}
>
```

除了find()方法还有findOne()方法，仅返回一个文档。

##### RDBMS Where子句等效于MongoDB

查询文档在一些条件的基础上，可以使用下面的操作

|   操作  |   语法  |  示例   |  RDBMS等效语句   |
| --- | --- | --- | --- |
|  Equality   |  {<key>:<value>}   |   db.mycol.find({"by":"yiibai tutorials"}).pretty()  |  where by 等于 'yiibai tutorials'   |
|   Less Than  |  <key>:{$lt:<value>}}   |  db.mycol.find({"likes":{$lt:50}}).pretty()   |  where likes 小于 50  |
|  Less Than Equals   |  {<key>:{$lte:<value>}}   |  db.mycol.find({"likes":{$lte:50}}).pretty()   | where likes 小于等于 50     |
|  Greater Than   |   {<key>:{$gt:<value>}}  |   db.mycol.find({"likes":{$gt:50}}).pretty()  |   where likes 大于 50  |
|  Greater Than Equals   |  {<key>:{$gte:<value>}}   |   db.mycol.find({"likes":{$gte:50}}).pretty()  |   where likes 大于等于 50  |
|  Not Equals   |   {<key>:{$ne:<value>}}  | db.mycol.find({"likes":{$ne:50}}).pretty()    |  where likes 不等于 50   |


##### AND 在 MongoDB

##### 语法

在 find()方法，如果您传递多个键通过","将它们分开，那么MongoDB对待它就如AND条件一样。基本语法如下所示：

``` javascript
>db.mycol.find({key1:value1, key2:value2}).pretty()
```

##### 例子

下面给出的例子将显示所有教程含“yiibai tutorials”和其标题是“MongoDB Overview”

``` javascript
>db.mycol.find({"by":"yiibai tutorials","title": "MongoDB Overview"}).pretty()
{
   "_id": ObjectId(7df78ad8902c),
   "title": "MongoDB Overview", 
   "description": "MongoDB is no sql database",
   "by": "yiibai tutorials",
   "url": "http://www.yiibai.com",
   "tags": ["mongodb", "database", "NoSQL"],
   "likes": "100"
}
>
```

对于上面给出的例子相当于where子句：' where by='yiibai tutorials' AND title='MongoDB Overview' '。可以传递任何数目的键-值对在find子句。

##### OR 在 MongoDB

##### 语法

要查询基于OR条件的文件，需要使用$or关键字。OR的基本语法如下所示：

``` javascript
>db.mycol.find(
   {
      $or: [
	     {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```

##### 例子

下面给出的例子将显示所有撰写含有 'yiibai tutorials' 或是标题为 'MongoDB Overview' 的教程

``` javascript
>db.mycol.find({$or:[{"by":"tutorials point"},{"title": "MongoDB Overview"}]}).pretty()
{
   "_id": ObjectId(7df78ad8902c),
   "title": "MongoDB Overview", 
   "description": "MongoDB is no sql database",
   "by": "yiibai tutorials",
   "url": "http://www.yiibai.com",
   "tags": ["mongodb", "database", "NoSQL"],
   "likes": "100"
}
>
```

##### 使用 AND 和 OR 在一起

##### 例子

下面给出的例子显示有喜欢数大于100 的文档，其标题要么是 'MongoDB Overview' 或 'yiibai tutorials'. 等效于SQL的where子句：**'where likes>10 AND (by = 'yiibai tutorials' OR title = 'MongoDB Overview')'**

``` javascript
>db.mycol.find("likes": {$gt:10}, $or: [{"by": "yiibai tutorials"}, {"title": "MongoDB Overview"}] }).pretty()
{
   "_id": ObjectId(7df78ad8902c),
   "title": "MongoDB Overview", 
   "description": "MongoDB is no sql database",
   "by": "yiibai tutorials",
   "url": "http://www.yiibai.com",
   "tags": ["mongodb", "database", "NoSQL"],
   "likes": "100"
}
>
```

## 更新文档

MongoDB的update()和save()方法用于更新文档到一个集合。 update()方法将现有的文档中的值更新，而save()方法使用传递到save()方法的文档替换现有的文档。

##### MongoDB Update() 方法

##### 语法

update()方法的基本语法如下

``` javascript
>db.COLLECTION_NAME.update(SELECTIOIN_CRITERIA, UPDATED_DATA)
```

##### 例子

考虑mycol集合有如下数据。

``` javascript
{ "_id" : ObjectId(5983548781331adf45ec5), "title":"MongoDB Overview"}
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Yiibai Yiibai Overview"}
```

下面的例子将设置其标题“MongoDB Overview”的文件为新标题为“New MongoDB Tutorial”

``` javascript
>db.mycol.update({'title':'MongoDB Overview'},{$set:{'title':'New MongoDB Tutorial'}})
>db.mycol.find()
{ "_id" : ObjectId(5983548781331adf45ec5), "title":"New MongoDB Tutorial"}
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Yiibai Tutorial Overview"}
>
```

默认情况下，MongoDB将只更新单一文件，更新多，需要一个参数 'multi' 设置为 true。

``` javascript
>db.mycol.update({'title':'MongoDB Overview'},{$set:{'title':'New MongoDB Tutorial'}},{multi:true})
```

##### MongoDB Save() 方法

save() 方法取代，通过新文档到 save()方法

##### 语法

mongodb 的 save()方法如下所示的基本语法：

``` javascript
>db.COLLECTION_NAME.save({_id:ObjectId(),NEW_DATA})
```

##### 例子

下面的例子将替换该文件_id '5983548781331adf45ec7'

``` javascript
>db.mycol.save(
   {
      "_id" : ObjectId(5983548781331adf45ec7), "title":"Yiibai Yiibai New Topic", "by":"Yiibai Yiibai"
   }
)
>db.mycol.find()
{ "_id" : ObjectId(5983548781331adf45ec5), "title":"Yiibai Yiibai New Topic", "by":"Yiibai Yiibai"}
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Yiibai Yiibai Overview"}
>
```

## 删除文档

MongoDB 的 **remove()**方法用于从集合中删除文档。remove()方法接受两个参数。一个是标准缺失，第二是justOne标志

- **deletion criteria** : 根据文件（可选）删除条件将被删除。
- **justOne** : （可选）如果设置为true或1，然后取出只有一个文档。

##### 语法

**remove()**方法的基本语法如下

``` javascript
>db.COLLECTION_NAME.remove(DELLETION_CRITTERIA)
```

##### 例子

考虑mycol集合有如下数据。

``` javascript
{ "_id" : ObjectId(5983548781331adf45ec5), "title":"MongoDB Overview"}
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Yiibai Yiibai Overview"}
```

下面的例子将删除所有的文件，其标题为 'MongoDB Overview'

``` javascript
>db.mycol.remove({'title':'MongoDB Overview'})
>db.mycol.find()
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Yiibai Toturials Overview"}
>
```

##### 只删除一个

如果有多个记录，并要删除仅第一条记录，然后在 remove()方法设置参数 justOne 。

``` javascript
>db.COLLECTION_NAME.remove(DELETION_CRITERIA,1)
```

##### 删除所有文件

如果没有指定删除条件，则MongoDB将从集合中删除整个文件。这相当于SQL的 truncate 命令。

``` javascript
>db.mycol.remove()
>db.mycol.find()
>
```

## MongoDB投影

mongodb投影意义是只选择需要的数据，而不是选择整个一个文档的数据。如果一个文档有5个字段，只需要显示3个，只从中选择3个字段。

MongoDB的find()方法，解释了MongoDB中查询文档接收的第二个可选的参数是要检索的字段列表。在MongoDB中，当执行find()方法，那么它会显示一个文档的所有字段。要限制这一点，需要设置字段列表值为1或0。1是用来显示字段，而0被用来隐藏字段。

##### 语法

find()方法的基本语法如下

``` javascript
>db.COLLECTION_NAME.find({},{KEY:1})
```

##### 例子

考虑集合 **myycol** 有下列数据

``` javascript
{ "_id" : ObjectId(5983548781331adf45ec5), "title":"MongoDB Overview"}
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Yiibai Yiibai Overview"}
```

下面的例子将显示文档的标题，在查询文档时。

``` javascript
>db.mycol.find({},{"title":1,_id:0})
{"title":"MongoDB Overview"}
{"title":"NoSQL Overview"}
{"title":"Yiibai Yiibai Overview"}
>
```

**请注意在执行find()方法时_id字段始终显示，如果不想要显示这个字段，那么需要将其设置为0**

## 限制文档 

##### MongoDB Limit() 方法

要在MongoDB中限制记录，需要使用limit()方法。 limit() 方法接受一个数字类型的参数，这是要显示的文档数量。

##### 语法

limit()方法的基本语法如下

``` javascript
>db.COLLECTION_NAME.find().limit(NUMBER)
```

##### 例子

考虑集合 myycol 有下列数据

``` javascript
{ "_id" : ObjectId(5983548781331adf45ec5), "title":"MongoDB Overview"}
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Yiibai Yiibai Overview"}
```

下面的例子将只显示2个文档，在查询文档时。

``` javascript
>db.mycol.find({},{"title":1,_id:0}).limit(2)
{"title":"MongoDB Overview"}
{"title":"NoSQL Overview"}
>
```

如果不指定 **limit()** 方法的参数数量，然后它会显示集合中的所有文档。

##### MongoDB Skip() 方法

除了 **limit()** 方法还有一个方法 **skip()** 也接受数字类型参数并用于跳过文件数。

##### 语法

skip() 方法的基础语法如下所示：

``` javascript
>db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
```

##### 例子

下面的例子将仅显示第二个文档。

``` javascript
>db.mycol.find({},{"title":1,_id:0}).limit(1).skip(1)
{"title":"NoSQL Overview"}
>
```

**请注意，skip() 方法的默认值是 0**

## 文档排序

要排序MongoDB中的文档，需要使用 sort()方法。 sort() 方法接受一个包含字段列表以及排序顺序的文档。 要使用1和-1指定排序顺序。1用于升序，而-1是用于降序。

##### 语法

sort()方法的基本语法如下

``` javascript
>db.COLLECTION_NAME.find().sort({KEY:1})
```

##### 例子

考虑集合 myycol 有如下数据

``` javascript
{ "_id" : ObjectId(5983548781331adf45ec5), "title":"MongoDB Overview"}
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Yiibai Yiibai Overview"}
```

下面的例子将显示的文件排序按标题降序排序。

``` javascript
>db.mycol.find({},{"title":1,_id:0}).sort({"title":-1})
{"title":"Yiibai Yiibai Overview"}
{"title":"NoSQL Overview"}
{"title":"MongoDB Overview"}
>
```

**请注意，如果不指定排序类型，那么 sort() 方法将以升序排列文档。**

## MongoDB索引

索引支持查询高效率执行。如果没有索引，MongoDB必须扫描集合中的每一个文档，然后选择那些符合查询语句的文档。若需要 mongod 来处理大量数据，扫描是非常低效的。

索引是特殊的数据结构，存储在一个易于设置遍历形式的数据的一小部分。索引存储在索引中指定特定字段的值或一组字段，并排序字段的值。

要创建索引，需要使用MongoDB的ensureIndex()方法。

##### 语法

ensureIndex()方法的基本语法如下

``` javascript
>db.COLLECTION_NAME.ensureIndex({KEY:1})
```

这里键是要创建索引字段，1是按名称升序排序。若以按降序创建索引，需要使用 -1.

##### 例子

``` javascript
>db.mycol.ensureIndex({"title":1})
>
```

在 ensureIndex()方法，可以通过多个字段，来创建多个字段索引。

``` javascript
>db.mycol.ensureIndex({"title":1,"description":-1})
>
```

**ensureIndex()** 方法还接受选项列表（这是可选），其列表如下：

|   参数  |  类型   |   描述  |
| --- | --- | --- |
|   background  |   Boolean  |  构建索引在后台以便建立索引不阻止其它数据库活动。指定true时建立在后台。缺省值是false.   |
|  unique   |   Boolean  |  创建一个唯一的索引，以使集合将不接受插入的的文档，其中的索引关键字或键匹配索引的现有值。指定true以创建唯一索引。缺省值是 false.   |
|  name   |   string  |  	索引的名称。如果未指定，MongoDB通过连接索引的字段和排序顺序的名称生成一个索引名。   |
|  dropDups   |   Boolean  |   创建一个字段唯一索引时可能会有重复。MongoDB索引键仅第一次出现，并从集合中删除包含该键后续出现的所有文档。指定true以创建唯一索引。缺省值是 false.  |
|  sparse   |  Boolean   |   如果为true，索引只引用与指定的字段的文档。这些索引使用更少的空间，但在某些情况下表现不同（特别是排序）。缺省值是 false.  |
|  expireAfterSeconds   |  integer   |   指定的值，以秒为单位，作为一个TTL控制MongoDB保留在此集合文件多久。  |
|   v  |  index version   |   索引版本号。默认的索引版本取决于mongod创建索引时运行的版本。  |
|   weights  |  document  |  重量（weight ）是一个数字，它是从1至99,999的数字，表示字段相对于其它索引字段在得分方面的意义。   |
|   default_language  |   string  |   对于文本索引，并为词干分析器和标记生成器列表中的语言决定了停用词和规则。它的默认值： english.  |
|   language_override  |  string   |  对于一个文本索引，包含在文档中指定字段的名称，语言来覆盖默认语言。它的默认值：language.   |

## MongoDB 聚合

聚合操作处理数据记录并返回计算结果。从多个文档聚合分组操作数值，并可以执行多种对分组数据业务返回一个结果。 在SQL中的count(*)，使用group by 与mongodb的聚合是等效的。 对于MongoDB的聚合，使用的是aggregate()方法。

##### 语法

aggregate()方法的基本语法如下

``` javascript
>db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)
```

##### 例子

在集合中有以下数据：

``` javascript
{
   _id: ObjectId(7df78ad8902c)
   title: 'MongoDB Overview', 
   description: 'MongoDB is no sql database',
   by_user: 'Yiibai Yiibai ',
   url: 'http://www.yiibai.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 100
},
{
   _id: ObjectId(7df78ad8902d)
   title: 'NoSQL Overview', 
   description: 'No sql database is very fast',
   by_user: 'Yiibai Yiibai',
   url: 'http://www.yiibai.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 10
},
{
   _id: ObjectId(7df78ad8902e)
   title: 'Neo4j Overview', 
   description: 'Neo4j is no sql database',
   by_user: 'Neo4j',
   url: 'http://www.neo4j.com',
   tags: ['neo4j', 'database', 'NoSQL'],
   likes: 750
}
```

现在从上面的集合，如果想知道每一个用户编写的教程是多少，那么使用aggregate()方法，如下图所示的列表：

``` javascript
> db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : 1}}}])
{
   "result" : [
      {
         "_id" : "Yiibai Yiibai",
         "num_tutorial" : 2
      },
      {
         "_id" : "Neo4j",
         "num_tutorial" : 1
      }
   ],
   "ok" : 1
}
>
```

用于上述用途将等效于sql查询： select by_user, count(*) from mycol group by by_user

另外，在上述例子中，我们已经使用字段by_user进行分组并计算总和，也就是by_user 出现各个次数。一个列表中可用的聚集表达式。

|  表达式  |   描述  |   示例  |
| --- | --- | --- |
|  $sum   |   从集合累加所有文档中的定义值  |  db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}])   |
|  $avg   |  从集合中的所有文档计算所有给定值的平均值   |  db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}])  |
|   $min  | 从集合中获取的所有文件的最小的相应值    |   	db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}])  |
|   $max  |  从集合中的所有文档中的相应值中获取最大值   |  db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}])   |
|  $push   |  插入数组值到文档中   |  	db.mycol.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}])   |
|  $addToSet   |   插入值所产生的数组到文档中，但不会产生重复  |  db.mycol.aggregate([{$group : {_id : "$by_user", url : {$addToSet : "$url"}}}])   |
|  $first   |  从源文件获取根据分组的头文件。通常，这使得只能意会再加上一些以前应用“$sort” -stage    |  db.mycol.aggregate([{$group : {_id : "$by_user", first_url : {$first : "$url"}}}])   |
|   $last  |  从源文件获取根据分组的最后文件。通常，这使得只能意会再加上一些以前应用 “$sort”-stage.   |   db.mycol.aggregate([{$group : {_id : "$by_user", last_url : {$last : "$url"}}}])  |

## MongoDB 复制

复制是同步在多个服务器上的数据过程。复制提供了冗余和数据在不同的数据库服务器上的多个副本提高数据的可用性，复制防止在单个服务器上丢失数据库。 复制也可以从硬件故障和服务中断中恢复。带有数据的其他副本，可以选择其中一个灾难恢复，报告或备份。

为什么要复制？

- 为了让数据安全
- 数据的高（24*7）可用性
- 灾难恢复
- 无停机维护（如备份，索引重建，压缩）
- 读取缩放（额外的副本来读取）
- 副本集是透明的应用

##### MongoDB复制的工作原理

MongoDB通过使用副本集的复制来实现。副本集是一组承载同一个数据集的mongod实例。在副本的一个节点是接收所有的写操作主节点。所有的实例，次级，应用操作从主以便它们具有相同的数据集。副本集只能有一个主节点。

- 副本集是一组两个或更多个节点（通常至少3节点是必需的）。
- 在副本集一个节点是主节点和其余的节点都是次要的。
- 所有的数据复制是从主到次节点。
- 在自动故障转移或维护时，选建立了主要和一个新的主节点被选择。
- 故障节点的恢复后，再次加入副本集，并可以作为一个辅助节点。

mongodb复制的典型图如下图，其中客户端应用程序总是与主节点和主节点交互，然后将数据复制到辅助节点。

![](http://ww1.sinaimg.cn/large/b3ad6cffly1g25gk1gxifj20dw0bd74v.jpg)

##### 副本集特征

- N个节点的集群
- 任何节点可为原发/主节点
- 所有的写操作进入到主节点
- 自动故障转移
- 自动恢复
- 协商一致选择主节点

##### 建立一个副本集

在本教程中，我们将独立的 mongod 实例转换为副本集。 要转换为副本集，按照以下的步骤：

- 关闭已经运行的 MongoDB 服务器。
- 现在，通过指定--replSet选项启动 MongoDB 服务器。--replSet 的基本语法如下：

-	``` javascript
	mongod --port "PORT" --dbpath "YOUR_DB_DATA_PATH" --replSet "REPLICA_SET_INSTANCE_NAME"
	```
	
-	``` javascript
	mongod --port 27017 --dbpath "D:\software\MongoDB\Server\3.0\mongodb\data" --replSet rs0
	```

- 这将启动一个名为rs0的一个mongod实例，端口为： 27017
- 现在打开启动命令提示符，然后连接到mongod实例
- 在Mongo的客户端使用命令rs.initiate()来启动一个新的副本集
- 要检查副本设置配置，则使用命令rs.conf()
- 要检查副本集发行的状态，使用命令rs.status()

## MongoDB创建备份

##### MongoDB数据转储

要使用 mongodump 命令来执行 MongoDB 数据库备份。此命令将转储服务器的所有数据到转储目录。有许多可用的选项，通过它可以限制数据量或创建远程服务器备份。

##### 语法

mongodump命令的基本语法如下

``` javascript
>mongodump
```

##### 例子

启动 mongod 服务器。假设 mongod 服务器运行在本地主机和端口 27017. 现在打开一个命令提示符，然后转到你的MongoDB实例的bin目录，然后输入命令mongodump。

考虑mycol集合有以下数据。

``` javascript
>mongodump
```

该命令将连接到服务器127.0.0.1和端口27017，并备份所有数据到服务器上的目录： /bin/dump/. 命令的输出如下所示：

![](http://ww1.sinaimg.cn/large/b3ad6cffly1g25gpmm3kij20it08aaar.jpg)

以上是可用的选项能够与mongodump命令一起使用的列表。

此命令将只备份指定数据库到指定的路径

|   语法  |   描述  |  示例   |
| --- | --- | --- |
|  mongodump --host HOST_NAME --port PORT_NUMBER   |  这个命令将备份指定的mongod实例的所有数据库   |  mongodump --host yiibai.com --port 27017  |
|  mongodump --dbpath DB_PATH --out BACKUP_DIRECTORY   |     |  mongodump --dbpath /data/db/ --out /data/backup/   |
|   mongodump --collection COLLECTION --db DB_NAME  |   此命令将仅备份指定的特定数据库集合  |  mongodump --collection mycol --db test   |

##### 数据恢复

要恢复备份的MongoDB数据，则使用mongorestore命令。该命令将从备份目录恢复所有的数据。

##### 语法

mongorestore命令的基本语法

``` javascript
>mongorestore
```

这个命令的输出如下所示：

![](http://ww1.sinaimg.cn/large/b3ad6cffly1g25gsbqq7cj20it0cq0tu.jpg)