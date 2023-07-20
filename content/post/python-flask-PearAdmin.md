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


## Python���е�ѡ��

### APScheduler

- ���ڼ�, pass��


### Celery

- ���й�ҵǿ�ȵ�
- �����������Ӧ��, ѧϰ���Դ�ɱ�����
- ��ά������Ҳ���϶�


### Redis

- Unix�����ѧ: Do one thing and do it well
- ֻ�����ײ���кͻ���


### RQ(Redis Queue)

- RQ (Redis Queue) is a simple Python library for queueing jobs and processing them in the background with workers.


### ThreadPoolExecutor

- �Զ��߳�/����̵ķ�װ, ���ø���




## APSCHEDULER

- Pythonʹ��APSchedulerʵ�ֶ�ʱ����  [����](https://www.cnblogs.com/gdjlc/p/11432526.html)

- Python��ʱ����-APScheduler  [����](https://blog.csdn.net/wuyongpeng0912/article/details/88895659)




## CELERY

- flask + celery + redis  [����](https://blog.csdn.net/blackj_liuyun/article/details/79691701?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.essearch_pc_relevant&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.essearch_pc_relevant)

- flask��ʹ��celeryʵ���첽����  [����](https://blog.csdn.net/weixin_40612082/article/details/81149592?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.no_search_link)

- celery�����ֲ�  [����](https://www.celerycn.io)




## REDIS

- python������redis�����������(queue)  [����](https://www.cnblogs.com/arkenstone/p/7813551.html)

- python redis����Դ�뼰Ӧ�÷���  [����](https://www.jianshu.com/p/32bf08e885b0)

- redis�ο��ֲ�  [����](https://www.runoob.com/redis/redis-backup.html)

- python3 ���� redis List(�б�) ʵ�� ���  [����](https://blog.csdn.net/dangsh_/article/details/79221328)

- python������redis�����������(queue)  [����](https://www.cnblogs.com/arkenstone/p/7813551.html)




## REDIS QUEUE

- RQ����  [����](https://python-rq.org/)

- ΪFlask���ж��η�װ��RQ: Flask-RQ2  [����](https://github.com/rq/Flask-RQ2)


## CONCURRENT

- ���߳�, �����, �̳߳�  [����](https://blog.csdn.net/somezz/article/details/80963760)
    + CPU�ܼ��Ͷ����, ���߳�
    + I/O�ܼ��Ͷ����, ���߳�
    + �߳�ͬ��
        * Lock
        * Semaphore
        * Condition
        * Event
        * Queue

- ��ʹ���첽  [����](https://zhuanlan.zhihu.com/p/30897711)

- Python�̳߳� ThreadPoolExecutor ���÷���ʵս  [����](https://blog.csdn.net/weixin_30817749/article/details/100091835?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-3.highlightwordscore&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-3.highlightwordscore)

- ������ + Э��, ����� + Э��  [����](https://www.zhihu.com/question/23153641/answer/1592612272)
    + PearAdminFlask��production�������ڽű��е���Gunicorn

- Gunicorn VS uWSGI VS Uvicorn  [����1](https://medium.com/django-deployment/which-wsgi-server-should-i-use-a70548da6a83)  [����2](https://www.cnblogs.com/LeoGIS/p/14107024.html)  
    ����: ʹ��Gunicorn, ��PearAdminFlask��ʹ�õ���һ����
