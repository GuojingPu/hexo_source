---
title: python3 与 Django 连接数据库Error loading MySQL db module解决方案
tags:
  - Python
  - Django
  - MySQL
categories:
  - 技术
  - 数据库
abbrlink: 30439
date: 2018-09-30 17:11:41
---

学习Django时候，采用MySQL作为后台数据库，在执行
`python3 manage.py startapp appname`
创建app时遇到如下问题:

```
django.core.exceptions.ImproperlyConfigured: Error loading MySQLdb module.
Did you install mysqlclient?
```
完整报错如下：

```
Traceback (most recent call last):
  File "/usr/local/lib/python3.6/site-packages/django/db/backends/mysql/base.py", line 15, in <module>
    import MySQLdb as Database
ModuleNotFoundError: No module named 'MySQLdb'

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "manage.py", line 15, in <module>
    execute_from_command_line(sys.argv)
  File "/usr/local/lib/python3.6/site-packages/django/core/management/__init__.py", line 381, in execute_from_command_line
    utility.execute()
  File "/usr/local/lib/python3.6/site-packages/django/core/management/__init__.py", line 357, in execute
    django.setup()
  File "/usr/local/lib/python3.6/site-packages/django/__init__.py", line 24, in setup
    apps.populate(settings.INSTALLED_APPS)
  File "/usr/local/lib/python3.6/site-packages/django/apps/registry.py", line 112, in populate
    app_config.import_models()
  File "/usr/local/lib/python3.6/site-packages/django/apps/config.py", line 198, in import_models
    self.models_module = import_module(models_module_name)
  File "/usr/local/Cellar/python3/3.6.3/Frameworks/Python.framework/Versions/3.6/lib/python3.6/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 994, in _gcd_import
  File "<frozen importlib._bootstrap>", line 971, in _find_and_load
  File "<frozen importlib._bootstrap>", line 955, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 665, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 678, in exec_module
  File "<frozen importlib._bootstrap>", line 219, in _call_with_frames_removed
  File "/usr/local/lib/python3.6/site-packages/django/contrib/auth/models.py", line 2, in <module>
    from django.contrib.auth.base_user import AbstractBaseUser, BaseUserManager
  File "/usr/local/lib/python3.6/site-packages/django/contrib/auth/base_user.py", line 47, in <module>
    class AbstractBaseUser(models.Model):
  File "/usr/local/lib/python3.6/site-packages/django/db/models/base.py", line 101, in __new__
    new_class.add_to_class('_meta', Options(meta, app_label))
  File "/usr/local/lib/python3.6/site-packages/django/db/models/base.py", line 304, in add_to_class
    value.contribute_to_class(cls, name)
  File "/usr/local/lib/python3.6/site-packages/django/db/models/options.py", line 203, in contribute_to_class
    self.db_table = truncate_name(self.db_table, connection.ops.max_name_length())
  File "/usr/local/lib/python3.6/site-packages/django/db/__init__.py", line 33, in __getattr__
    return getattr(connections[DEFAULT_DB_ALIAS], item)
  File "/usr/local/lib/python3.6/site-packages/django/db/utils.py", line 202, in __getitem__
    backend = load_backend(db['ENGINE'])
  File "/usr/local/lib/python3.6/site-packages/django/db/utils.py", line 110, in load_backend
    return import_module('%s.base' % backend_name)
  File "/usr/local/Cellar/python3/3.6.3/Frameworks/Python.framework/Versions/3.6/lib/python3.6/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "/usr/local/lib/python3.6/site-packages/django/db/backends/mysql/base.py", line 20, in <module>
    ) from err
django.core.exceptions.ImproperlyConfigured: Error loading MySQLdb module.
Did you install mysqlclient?
```
原因分析：
在 python2 中，使用 pip install mysql-python  进行安装连接MySQL的库，使用时 import MySQLdb 进行使用

在 python3 中，改变了连接库，改为了 pymysql 库，但是在 Django 中， 连接数据库时使用的是 MySQLdb 库，这在与 python3 的合作中就会报以下错误了：

```
django.core.exceptions.ImproperlyConfigured: Error loading MySQLdb module: No module named 'MySQLdb'
```

使用pip install pymysql 进行安装，直接导入即可使用。
#### 1. 安装pymsql

```
pip install pymysql
```

#### 2. 安装完毕，打开项目的_init_.py,添加代码

```
import pymysql 
pymysql.install_as_MySQLdb()

```

重新执行`python3 manage.py startapp appname`创建app成功。

