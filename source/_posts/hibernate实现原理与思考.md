---
title: hibernate实现原理与思考
date: 2016-07-08 21:33:41
tags: Hibernate
---
#### hibernate概述(参考[Wikipedia](https://zh.wikipedia.org/wiki/Hibernate))
+ hibernate是一种Java语言下对象关系映射(ORM)解决方案；
+ 设计目标是将软件开发人员从大量相同的数据持久层相关编程工作中解放出来
+ hibernate可以想象成为一个中间组件；当Java通过SQL连接数据库的时候，hibernate负责将java程序的SQL语句接受过来发送带数据库处理，而数据库返回的数据，hibernate接受之后直接生成对象的形式传递给Java程序
___
#### 以一次数据库操作为例，分析hibernate实现原理.
+ 当SQL语句能正常执行：`select * from user;`
+ 项目启动时候，hibernate配置文件(`*.cfg.xml` & `user.hbm.xml`) 中的内容已经配置好在容器中，存储着实体类(`User`)与表的对应关系
+ 执行hql语句的时候，hibernate会根据反射机制先找到User的全路径名称，进而找到容器中User对应的配置
+ 由于对象的实体属性与表中的属性一一对应，hibernate会将HQL语句，根据不同的数据库方言解析为SQL语句，并在数据库中执行语句
___
#### 总的来说：
+ hibernate是将实体类中的字段按照`*.hbm.xml`配置文件或者是annotation的注释标记来解析一条或者多条sql语句，然后放在数据库中执行
1. 通过`configuration().configure();`读取并解析hibernate.cfg.xml配置文件
2. 执行完`hibernate.cfg.xml`的基本数据库映射配置之后，再通过配置文件中的 mapping 标记，解析并读取映射信息
3. `config.biuldSessionFactory();`创建SessionFactory;
4. `SessionFactory.open();` 打开Session
5. `session.beginTransaction();`创建事务
6. persistent operate 持久化操作
7. `session.getTransaction().commit();`提交事务
8. `session.getTransaction.rollback();`事务提交出错的时候，回滚事务
9. `session.close();`关闭事务
10. `SessionFactory.close();`

+ 这就是hibernate的原理(也可以叫流程)：基于一个ORM的主流持久化框架，对JDBC访问数据库代买进行了封装
___
####思考：
+ 昨天去了某厂面试，当面试官问我hibernate知不知道hibernate底层原理的时候,我说了不了解....
+ 因为在这个问题之前我就是已经简单的描述了hibernate的流程，当我听到底层原理的时候，及懵逼了，用了hibernate这么久，还真好像没看到过hibernate的底层原理的描述...
+ 回来查了一下,其实就是详细的描述hibernate流程...
+ 面试的时候，由于是第一次面试，紧张！还没适应面试的过程是怎么样的，应聘者应该如果主动的跟面试官沟通，如果将自己的能力以及个性给面试官展现出来...
+ 又是一次新鲜的经历，在各种经历中成长 @.@
