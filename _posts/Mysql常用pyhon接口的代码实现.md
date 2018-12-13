---
title: Mysql常用pyhon接口的实现：增、删、改、查
tags:
  - 数据库
  - Python
  - MySQL
categories:
  - 技术
  - 数据库
abbrlink: 581
date: 2018-08-28 23:20:31
---

代码如下：


```
#!/usr/bin/python3
# -*- coding:utf-8 -*-

import pymysql


"""
函数：CreateDatabase(Database_name)
功能：创建数据库
参数：Database_name：要创建的数据库名
"""
def CreateDatabase(Database_name):

    db = pymysql.connect(host='localhost',user='root',password='password',port=3306)
    cursor = db.cursor()
    cursor.execute('SELECT VERSION()')
    data = cursor.fetchone()
    print('DataBase Version:',data)

    sql = 'CREATE DATABASE {} DEFAULT CHARACTER SET utf8'.format(Database_name)
    try:
        cursor.execute(sql)
    except Exception as e:
        print('Create %s error：'%(Database_name), e)

    db.close()


"""
函数：CreateTables(DataBase,table)
功能：在指定数据库里面创建指定的表
参数：1.DataBase：已经创建的数据库名
     2.table：要创建的表名
"""
def CreateTables(DataBase,table):

    db = pymysql.connect(host='localhost', user='root', password='password', port=3306,db=DataBase)
    cursor = db.cursor()

    sql = 'CREATE TABLE IF NOT EXISTS {} (id VARCHAR(255) NOT NULL,user_name VARCHAR(255) NOT NULL, age INT NOT NULL, PRIMARY KEY (id))'.format(table)

    try:
        cursor.execute(sql)
    except Exception as e:
        print('%s Create %s error：'%(DataBase,table),e)

    db.close()


"""
函数：Insert(DataBase,table,data)
功能：在某一数据库的指定表中插入一条数据,如果该数据的key已经存在则会插入失败
参数：1.DataBase：已经创建的数据库名
     2.table：要创建的表名
     3.data：要插入的数据，字典类型比如：  data = {
                                            'id':'20180001',
                                            'user_name':'Bob',
                                            'age':20
                                        }
"""
def Insert(DataBase,table,data):

    db = pymysql.connect(host='localhost', user='root', password='password', port=3306, db=DataBase)
    cursor = db.cursor()

    keys = ','.join(data.keys())#->id,user_name,age
    values = ','.join(['%s'] * len(data))#['a']*4 -> [a,a,a,a]  ','.join([a,a,a,a])->'a,a,a,a'

    sql = 'INSERT INTO {t} ({k}) VALUES({v})'.format(t=table,k=keys,v=values)#使用form格式化结果：INSERT INTO students (id,user_name,age) VALUES(%s,%s,%s)
    print(sql)
    try:

        cursor.execute(sql, tuple(data.values()))
        db.commit()
    except Exception as e:
        print("insert error", e)
        db.rollback()

    db.close()


"""
函数：Update(Database,table,data)
功能：根据key更新某一数据库的指定表中的某一条数据，如果key存在在更新数据，不存在则插入数据
参数：1.DataBase：已经创建的数据库名
     2.table：要创建的表名
     3.data：要更新或插入的数据，字典类型比如：  data = {
                                            'id':'20180002',
                                            'user_name':'lio',
                                            'age':22
                                        }
"""
def Update(Database,table,data):
    """
       data = {
               'id':'20180001',
               'user_name':'Bob',
               'age':20
           }
       """

    # table = 'students'
    db = pymysql.connect(host='localhost', user='root', password='password', port=3306, db=Database)
    cursor = db.cursor()

    keys = ','.join(data.keys())      # ->id,user_name,age
    values = ','.join(['%s'] * len(data))  # ['a']*4 -> [a,a,a,a]  ','.join([a,a,a,a])->'a,a,a,a'

    sql = 'INSERT INTO {t} ({k}) VALUES({v}) ON DUPLICATE KEY UPDATE'.format(t=table, k=keys, v=values)        # 使用format格式化INSERT INTO students (id,user_name,age) VALUES(%s,%s,%s) ON DUPLICATE KEY UPDATE
    update = ','.join(' {key} = %s'.format(key=key) for key in data)      #注意前面有空格，结果：' id = %s,user_name = %s,age = %s'

                       #ON DUPLICATE KEY UPDATE 表示如果主键存在就执行UPDATE 而不是插入
    sql += update      #结果：INSERT INTO students (id,user_name,age) VALUES(%s,%s,%s) ON DUPLICATE KEY UPDATE id = %s, user_name = %s, age = %s

    try:

        cursor.execute(sql, tuple(data.values())*2)#使用了2次值，共6个%s
        db.commit()
    except Exception as e:
        print("insert error", e)
        db.rollback()

    db.close()


"""
函数：Delete(Database,table,condition):
功能：根据指定条件删除数据库某表中的满足条件的数据
参数：1.DataBase：已经创建的数据库名
     2.table：要创建的表名
     3.condition：指定的SQL语句条件，比如：'age > 20',表示删除所有 age字段的值大于20的数据
"""
def Delete(Database,table,condition):

    db = pymysql.connect(host='localhost', user='root', password='password', port=3306, db=Database)
    cursor = db.cursor()

    sql = 'DELETE FROM {tables} WHERE {conditions}'.format(tables=table,conditions=condition)
    try:
        cursor.execute(sql)
        db.commit()
    except Exception as e:
        print("insert error", e)
        db.rollback()

    db.close()


"""
函数：Select(Database,table,condition)
功能：从数据库某表中根据指定条件查询的满足条件的数据
参数：1.DataBase：已经创建的数据库名
     2.table：要创建的表名
     3.condition：指定的SQL语句条件，比如：'age > 20',表示查询所有age字段的值大于20的数据
返回值：列表，存储满足查询条件的信息，列表的每个元素是一条信息，用元组封装。
"""
def Select(Database,table,condition):

    db = pymysql.connect(host='localhost', user='root', password='password', port=3306, db=Database)
    cursor = db.cursor()

    sql = 'SELECT * FROM {tablses} WHERE {conditions}'.format(tablses=table,conditions=condition)
    result = []
    try:
        cursor.execute(sql)
        row = cursor.fetchone()#row是以元组返回，row： ('20180001', 'Bob', 20)

        while row:
            result.append(row)
            row=cursor.fetchone()

    except Exception as e:
        print("SELECT error", e)

    db.close()
    return result




def main():

    CreateDatabase('test_db')

    CreateTables('test_db','test_db_table_1')

    data = {
            'id':'20180004',
            'user_name':'jack',
            'age':22
        }

    Insert('test_db','test_db_table_1',data)

    data = {
            'id':'20180004',
            'user_name':'jack',
            'age':25
        }

    Update('test_db','test_db_table_1',data)

    #Delete('test_db','test_db_table_1','age > 21')

    result= Select('test_db','test_db_table_1','age > 19')

    print(result)

if __name__ == '__main__':
    main()
```

