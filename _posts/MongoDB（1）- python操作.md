---
title: MongoDB（1）- 安装、python操作
tags:
  - 数据库
  - MongoDB
categories:
  - 技术
  - 数据库
abbrlink: 27048
date: 2018-08-29 17:22:49
---
## 介绍
MongoDB C++语言编写，基于分布式文件存储系统开源数据库，其内容存储形式类似JSON对象，他的字段值可以包含其他文档、数组已经文档数组，非常灵活。
[中文官网](https://www.mongodb.com/cn)
[官方文档](https://docs.mongodb.com/)


## 安装
### Windows
不建议，不使用,安装非常麻烦！
### Ubuntu
官方安装教程：https://docs.mongodb.com/manual/installation/

### Mac

安装：

```
brew install mongodb
```
创建存放数据库的文件夹：

```
mkdir /data/db
```
启动：


```
brew services start mongodb
sudo mongod
```
停止和重启：

```
brew services stop mongodb
brew services restart mongodb
```

参考其他:

````
#brew install mongodb
#新建一个文件夹/data/db 用于存放MangoDB数据
#启动MongoDB服务：brew services start mongodb
#sudo mongodb
#停止MongoDB服务：brew services stop mongodb
#重启MongoDB服务：brew services restart mongodb
````

**Mac最大的优势就是不管什么软件安装起来都是如此简单！！！！**


## python 下使用
### 安装pymongondb 包

```
pip3 install pymongo
```

### 使用

```
#!/usr/bin/python3
# -*- coding:utf-8 -*-
import pymongo
import pymongo

#创建MongoDB连接对象
client =  pymongo.MongoClient(host='localhost',port=27017)
#client = pymongo.MongoClient('mongondb://localhost:27017/')


#指定或创建数据库
db = client.testmongodb
#db = client['testmongodb']


#指定或创建集合(类似于table)
collection = db.studens
#collection = db['studens']


#插入数据，数据以字典的形势表示
data = {
    'id':'20180001',
    'name':'Lio',
    'age':20
}

#第一种方式，插入一条数据
#result = collection.insert(data)
#print(result)#5b866486623b564260fca3f7

data_1 = {
    'id':'20180002',
    'name':'mark',
    'age':21
}
data_2 = {
    'id':'20180003',
    'name':'kang',
    'age':24
}
#第一种方式，插入多条数据
#result = collection.insert([data_1,data_2])
#print(result)#[ObjectId('5b866b6c623b5643595070b2'), ObjectId('5b866b6c623b5643595070b3')]


data_3 = {
    'id':'20180004',
    'name':'lin',
    'age':25,
    'gender':'male'
}
#第二种方式，插入一条数据
#result = collection.insert_one(data_3)
#print(result)#<pymongo.results.InsertOneResult object at 0x10c1d3d88>
#print(type(result))#<class 'pymongo.results.InsertOneResult'>
#print(result.inserted_id)#5b866c5d623b5643904db0af


data_4 = {
    'id':'20180005',
    'name':'heler',
    'age':30,
    'gender':'male'
}

data_5 = {
    'id':'20180006',
    'name':'jay',
    'age':22,
    'gender':'female'
}
#第二种方式插入多条数据
#result = collection.insert_many([data_4,data_5])
#print(result)#<pymongo.results.InsertManyResult object at 0x109435ec8>
#print(type(result))#<class 'pymongo.results.InsertManyResult'>
#print(result.inserted_ids)#[ObjectId('5b866d0f623b5643c6acf070'), ObjectId('5b866d0f623b5643c6acf071')]

#####   推荐使用insert_one()和insert_many()

#查询
#可以使用find_one()和find（）方法进行查询，其中find_one()查询得到的是单条结果，find（）则是返回一个生成器对象

#result = collection.find_one({'name':'Lio'})
#print(type(result))#<class 'dict'>
#print(result)#{'_id': ObjectId('5b866486623b564260fca3f7'), 'id': '20180001', 'name': 'Lio', 'age': 20}

#返回的是字典类型，其中数据多了一个_id属性他是MongoDB插入数据时自动添加的
#此外我们可以根据ObjectId来查询，此时需要bson库中的objectid:
#from bson.objectid import ObjectId
#result = collection.find_one({'_id':ObjectId('5b866486623b564260fca3f7')})
#print(type(result))#<class 'dict'>
#print(result)#{'_id': ObjectId('5b866486623b564260fca3f7'), 'id': '20180001', 'name': 'Lio', 'age': 20}


#对于多条数据查询可以使用find
#results = collection.find({'age':30})
#print(type(results))#<class 'pymongo.cursor.Cursor'>
#print(results)#<pymongo.cursor.Cursor object at 0x10cd07160>
#for result in results:#{'_id': ObjectId('5b866d0f623b5643c6acf070'), 'id': '20180005', 'name': 'heler', 'age': 30, 'gender': 'male'}
#    print(result)

#返回的结果是cursor类型，相当于一个生成器


#条件查询
#查询年龄大于20的数据
#results = collection.find({'age':{'$gt':20}})
#print(type(results))#<class 'pymongo.cursor.Cursor'>
#print(results)#<pymongo.cursor.Cursor object at 0x10cf561d0>
#for result in results:#
#    print(result)
#查询结果：
#{'_id': ObjectId('5b866b6c623b5643595070b2'), 'id': '20180002', 'name': 'mark', 'age': 21}
#{'_id': ObjectId('5b866b6c623b5643595070b3'), 'id': '20180003', 'name': 'kang', 'age': 24}
#{'_id': ObjectId('5b866c5d623b5643904db0af'), 'id': '20180004', 'name': 'lin', 'age': 25, 'gender': 'male'}
#{'_id': ObjectId('5b866d0f623b5643c6acf070'), 'id': '20180005', 'name': 'heler', 'age': 30, 'gender': 'male'}
#{'_id': ObjectId('5b866d0f623b5643c6acf071'), 'id': '20180006', 'name': 'jay', 'age': 22, 'gender': 'female'}

#这里的查询条件是一个字典形式：意思是查询age大于20的信息,$gt是大于，其他常见入如下：
#$lt       小于        {'age':{'$lt':20}}
#$gt       大于        {'age':{'$gt':20}}
#$lte       小于等于        {'age':{'$lte':20}}
#$gte       大于等于        {'age':{'$gte':20}}
#$ne       不等于        {'age':{'$ne':20}}
#$in      在范围内        {'age':{'$in':[20,23]}}
#$nin      不在范围内        {'age':{'$nin':[20,23]}}
#$lt       小于        {'age':{'$t':20}}

#还可以正则匹配

#results = collection.find({'name':{'$regex':'^M.*'}})

#'$regex'表示使用正则匹配，'^M.*'表示以M开头
#其他：
#$exists       属性是否存在       {'name':{'$exists':True}}      name的属性存在
#$type         类型判断          {'age':{'$type':'int'}}        age的类型为int
#$mod          数字模操作        {'age':{'$mod':'[5,0]'}}       年龄的模5余0

#$text         文本查询          {'$text':{'$search':'Mike'}}   text类型的属性中包含Mike字符串
#$where        高级条件查询       {'where':'obj.fans_count == obj.follows_count'}       自身粉丝数等于关注数


#计数
#可使用count函数统计查询的数据条数

#count = collection.find({'name':{'$regex':'^.*'}}).count()
#print(count)


#排序
#pymongo.ASCENDIN升序,pymongo.DESCENDING降序
#results = collection.find().sort('name',pymongo.DESCENDING)
#for result in results:
#    print(result)

#偏移
#使用skip(2)，表示偏移几个位置，及忽略查询到的前面2条数据
#使用limit(2)表示最多获取多少结果，限制获取的条数
#！！！注意在庞大数据的时候（千万、亿级别）不要使用大偏移量来查询数据，可能导致内存溢出
#可以使用类似如下操作:
#from bson.objectid import ObjectId
#collection.find({'_id':{'$gt':'5b866b6c623b5643595070b2'}})
#这时只需要记录上次查询的id


#更新
#使用update()方法更新
#condition = {'name':'Lio'}
#result_1 = collection.find_one(condition)
#print(result_1)
#result_1['age'] = 20
#result_2 = collection.update(condition,result_1)
#print(result_2)

#这里我们需要先查询到要修改的数据，修改完之后使用udate()方法将修改后的数据传入
#执行结果
#{'_id': ObjectId('5b866486623b564260fca3f7'), 'id': '20180001', 'name': 'Lio', 'age': 20}
#{'n': 1, 'nModified': 0, 'ok': 1.0, 'updatedExisting': True}

#还可以使用 $set 进行更新,注意使用$set只会更新result_1字典里存在的字段，如果原先还有其他字段怎不会更新也不会删除
#而如果不用$set，会把之前的数据全部用result_1替换，若原数据存在其他字段则会被删除
#condition = {'name':'Lio'}
#result_1 = collection.find_one(condition)
#print(result_1)
#result_1['age'] = 20
#result_2 = collection.update(condition,{'$set':result_1})
#print(result_2)


#建议使用update_one()和update_many()，第二个参数不能直接使用字典要使用$类型操作符
#使用matched_count(匹配的数据条数)和modified_count(影响的数据条数)查看结果

#condition = {'name':'Lio'}
#result_1 = collection.find_one(condition)

#result_1['age'] = 22
#result_2 = collection.update_one(condition,{'$set':result_1})
#print(result_2)
#print(result_2.matched_count,result_2.modified_count)

#执行结果
#{'_id': ObjectId('5b866486623b564260fca3f7'), 'id': '20180001', 'name': 'Lio', 'age': 20}
#{'n': 1, 'nModified': 1, 'ok': 1.0, 'updatedExisting': True}
#<pymongo.results.UpdateResult object at 0x110dd2fc8>
#1 1


#使用条件
#表示查询年龄条件大于20的人，然后更新条件为{'$inc':{'age':1}，表示age加1，执行后将会在爱满足条件的数据age字段加1
#condition = {'age':{'$gt':20}}
#result_2 = collection.update_one(condition,{'$inc':{'age':1}})
#print(result_2)
#print(result_2.matched_count,result_2.modified_count)

#执行结果：由于使用了update_one所以只影响了1条数据
#<pymongo.results.UpdateResult object at 0x10627bf48>
#1 1

#使用update_many(),更新所有
#condition = {'age':{'$gt':10}}
#result_2s = collection.update_many(condition,{'$inc':{'age':1}})
#print(type(result_2s))
#print(result_2s)
#print(result_2s.matched_count,result_2s.modified_count)

#执行结果
#<class 'pymongo.results.UpdateResult'>
#<pymongo.results.UpdateResult object at 0x10f76bfc8>
#6 6



#删除
#result = collection.remove({'name':'heler'})
#print(result)   #{'n': 1, 'ok': 1.0}


#推荐使用delete_one()和delete_many()
#result = collection.delete_one({'name':'lin'})
#print(result)   #<pymongo.results.DeleteResult object at 0x10e8fadc8>
#print(result.deleted_count)


result = collection.delete_many({'age':{'$lt':26}})
print(result)   #<pymongo.results.DeleteResult object at 0x10e40bdc8>
print(result.deleted_count) #4


#其他操作
#find_one_and_delete()
#find_one_and_replace()
#find_one_and_update()
#详见官方文档
```


