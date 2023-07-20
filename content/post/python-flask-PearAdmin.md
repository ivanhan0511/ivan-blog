---
title: "Flask Basic"
date: 2022-05-24T16:57:10+08:00
categories:
- Python
- WebFramework
tags:
- Flask
keywords:
- flask
---

Some records of Flask
<!--more-->


## Python队列的选型

### APScheduler

- 过于简单, pass掉


### Celery

- 具有工业强度的
- 不过若想深度应用, 学习和试错成本过重
- 运维部署量也想多较多


### Redis

- Unix设计哲学: Do one thing and do it well
- 只用做底层队列和缓存


### RQ(Redis Queue)

- RQ (Redis Queue) is a simple Python library for queueing jobs and processing them in the background with workers.


### ThreadPoolExecutor

- 对多线程/多进程的封装, 调用更简单




## APSCHEDULER

- Python使用APScheduler实现定时任务  [链接](https://www.cnblogs.com/gdjlc/p/11432526.html)

- Python定时任务-APScheduler  [链接](https://blog.csdn.net/wuyongpeng0912/article/details/88895659)




## CELERY

- flask + celery + redis  [链接](https://blog.csdn.net/blackj_liuyun/article/details/79691701?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.essearch_pc_relevant&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.essearch_pc_relevant)

- flask中使用celery实现异步任务  [链接](https://blog.csdn.net/weixin_40612082/article/details/81149592?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link)

- celery中文手册  [链接](https://www.celerycn.io)




## REDIS

- python中利用redis构建任务队列(queue)  [链接](https://www.cnblogs.com/arkenstone/p/7813551.html)

- python redis事务源码及应用分析  [链接](https://www.jianshu.com/p/32bf08e885b0)

- redis参考手册  [链接](https://www.runoob.com/redis/redis-backup.html)

- python3 操作 redis List(列表) 实例 详解  [链接](https://blog.csdn.net/dangsh_/article/details/79221328)

- python中利用redis构建任务队列(queue)  [链接](https://www.cnblogs.com/arkenstone/p/7813551.html)




## REDIS QUEUE

- RQ官网  [链接](https://python-rq.org/)

- 为Flask进行二次封装的RQ: Flask-RQ2  [链接](https://github.com/rq/Flask-RQ2)


## CONCURRENT

- 多线程, 多进程, 线程池  [链接](https://blog.csdn.net/somezz/article/details/80963760)
    + CPU密集型多进程, 多线程
    + I/O密集型多进程, 多线程
    + 线程同步
        * Lock
        * Semaphore
        * Condition
        * Event
        * Queue

- 简单使用异步  [链接](https://zhuanlan.zhihu.com/p/30897711)

- Python线程池 ThreadPoolExecutor 的用法及实战  [链接](https://blog.csdn.net/weixin_30817749/article/details/100091835?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-3.highlightwordscore&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-3.highlightwordscore)

- 单进程 + 协程, 多进程 + 协程  [链接](https://www.zhihu.com/question/23153641/answer/1592612272)
    + PearAdminFlask的production环境是在脚本中调用Gunicorn

- Gunicorn VS uWSGI VS Uvicorn  [链接1](https://medium.com/django-deployment/which-wsgi-server-should-i-use-a70548da6a83)  [链接2](https://www.cnblogs.com/LeoGIS/p/14107024.html)  
    结论: 使用Gunicorn, 和PearAdminFlask中使用的是一样的
