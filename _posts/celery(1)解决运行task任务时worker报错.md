---
title: celery执行任务报错，有可能是你的版本不对
categories:
  - 技术
tags:
  - celery
comments: true
abbrlink: 28942
date: 2018-11-06 14:13:44
img:
---

如果的celery worker能够正常起来，但是在运行task任务时却显示如下错误:

```
(xxx-env) ➜  celerydemo celery -A celerydemo worker -l info
[2018-11-28 11:34:56,216: WARNING/MainProcess] /Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/celery/apps/worker.py:161: CDeprecationWarning:
Starting from version 3.2 Celery will refuse to accept pickle by default.

The pickle serializer is a security concern as it may give attackers
the ability to execute any command.  It's important to secure
your broker from unauthorized access when using pickle, so we think
that enabling pickle should require a deliberate action and not be
the default choice.

If you depend on pickle then you should set a setting to disable this
warning and to be sure that everything will continue working
when you upgrade to Celery 3.2::

    CELERY_ACCEPT_CONTENT = ['pickle', 'json', 'msgpack', 'yaml']

You must only enable the serializers that you will actually use.


  warnings.warn(CDeprecationWarning(W_PICKLE_DEPRECATED))

 -------------- celery@PgjMacbookPro.local v3.1.26.post2 (Cipater)
---- **** -----
--- * ***  * -- Darwin-18.2.0-x86_64-i386-64bit
-- * - **** ---
- ** ---------- [config]
- ** ---------- .> app:         celerydemo:0x10e621b70
- ** ---------- .> transport:   redis://127.0.0.1:6379/0
- ** ---------- .> results:     disabled://
- *** --- * --- .> concurrency: 4 (prefork)
-- ******* ----
--- ***** ----- [queues]
 -------------- .> celery           exchange=celery(direct) key=celery


[tasks]
  . main.tasks.mytask1

[2018-11-28 11:34:56,386: INFO/MainProcess] Connected to redis://127.0.0.1:6379/0
[2018-11-28 11:34:56,399: INFO/MainProcess] mingle: searching for neighbors
[2018-11-28 11:34:57,409: INFO/MainProcess] mingle: all alone
[2018-11-28 11:34:57,420: WARNING/MainProcess] /Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/celery/fixups/django.py:265: UserWarning: Using settings.DEBUG leads to a memory leak, never use this setting in production environments!
  warnings.warn('Using settings.DEBUG leads to a memory leak, never '
[2018-11-28 11:34:57,420: WARNING/MainProcess] celery@PgjMacbookPro.local ready.
[2018-11-28 11:36:06,000: ERROR/MainProcess] Unrecoverable error: AttributeError("'str' object has no attribute 'items'",)
Traceback (most recent call last):
  File "/Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/celery/worker/__init__.py", line 206, in start
    self.blueprint.start(self)
  File "/Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/celery/bootsteps.py", line 123, in start
    step.start(parent)
  File "/Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/celery/bootsteps.py", line 374, in start
    return self.obj.start()
  File "/Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/celery/worker/consumer.py", line 280, in start
    blueprint.start(self)
  File "/Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/celery/bootsteps.py", line 123, in start
    step.start(parent)
  File "/Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/celery/worker/consumer.py", line 884, in start
    c.loop(*c.loop_args())
  File "/Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/celery/worker/loops.py", line 76, in asynloop
    next(loop)
  File "/Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/kombu/async/hub.py", line 340, in create_loop
    cb(*cbargs)
  File "/Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/kombu/transport/redis.py", line 1019, in on_readable
    self._callbacks[queue](message)
  File "/Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/kombu/transport/virtual/__init__.py", line 534, in _callback
    self.qos.append(message, message.delivery_tag)
  File "/Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/kombu/transport/redis.py", line 146, in append
    pipe.zadd(self.unacked_index_key, delivery_tag, time()) \
  File "/Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/redis/client.py", line 2263, in zadd
    for pair in iteritems(mapping):
  File "/Users/pgj/.virtualenvs/xxx-env/lib/python3.6/site-packages/redis/_compat.py", line 123, in iteritems
    return iter(x.items())
AttributeError: 'str' object has no attribute 'items'
```


说明你的pip install  安装的redis 的版本不对！必须要和你的celery,celery-with-redis版本对应：
看一下我之前报错的redis版本：

```
redis             3.0.1
```
更改后的版本:

```
redis             2.10.6
```


