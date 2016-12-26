---
title: Mybatis 实现原理与结构解析
date: 2016-07-28 14:46:31
tags: Mybatis
description: MyBatis是目前非常流行的ORM框架,它的功能很强大,然而其实现却比较简单优雅;MyBatis 是支持普通 SQL查询，存储过程和高级映射的优秀持久层框架;MyBatis 消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索;MyBatis 使用简单的 XML或注解用于配置和原始映射,将接口和 Java 的POJOs(Plain Old Java Objects:普通的 Java对象)映射成数据库中的记录
---

### Mybatis概述
+ Mybatis是支持普通SQL查询，存储过程和高级映射的优秀持久层框架
+ 消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索
+ Mybatis使用简单的XML或注解用于配置和原始映射，将接口和Java的POJOs映射成数据库中的记录
+ 每个Mybatis应用程序都是基于SqlSessionFactory实例，而一个SqlSessionFactory实例可以通过SqlSessionFactoryBuilder读取xml配置文件或者一个预定义的配置类的实例获得

### Mybatis工作流程
1. 加载配置并初始化
+ 将SQL的配置信息加载成一个个`MappedStatement`对象(包括传入参数映射配置、执行SQL语句、结果映射配置)，存储在内存中
2. 接受调用请求
+ 触发条件：调用Mybatis提供的API
+ 传入参数：为SQL的ID和传入参数对象
+ 处理过程：将请求传递给下层的请求处理层进行处理
3. 处理触发请求
	+ 触发条件：API接口层传递请求过来
	+ 传入参数：SQL的ID和传入参数的对象
	+ 处理过程：
			     1. 根据SQL的ID查找对应的MappedStatement对象
			     2. 根据传入的参数对象解析MappedStatement对象，最终得到要执行的SQL和执行传入参数
			     3. 获取数据库连接，根据得到的最终SQL语句和执行传入参数到数据库执行，并得到执行结果
			     4. 根据MappedStatement对象中的结果映射配置对得到的执行结果进行转换处理，并得到最终的处理结果
			     5. 释放连接资源
4. 返回接受最终处理结果

### 工作原理结构
+ 功能架构：Mybatis的功能架构分为四层
	1. API接口层：提供给外部使用的API接口，开发人员通过这些本地的API来操纵数据库.接口层已收到调用请求就会调用数据处理层来完成具体的数据处理
	2. 数据处理层：负责具体的SQL查找，SQL解析，和结果执行映射处理等，其主要目的是根据调用的请求完成一次数据库的操作
	3. 基础支撑层：负责最基础的功能支撑，包括连接管理，事务管理，配置加载和缓存处理，这些都是共用的，将他们抽取出来作为最基础的封装组件，为上层的数据处理提供最基础的支撑
	4. 引导层：配置和启动Mybatis配置信息的方式，Mybatis提供两种方式来引导Mybatis：
	+ 基于XML配置文件获取配置信息
	+ 基于Java API的方式
<center>
![Mybatis功能架构](http://7xqvf0.com1.z0.glb.clouddn.com/%E5%8A%9F%E8%83%BD%E6%9E%B6%E6%9E%84.png)
</center>
+ 框架架构：
    1. 加载配置：配置来源于两个地方，一处是配置文件，一处是Java代码的注解，将SQL的配置信息加载为宜个MappedStatement对象(包括传入参数映射配置，执行的SQL语句，结果映射配置)　，存储在内存中
    2. SQL解析：当API接口层接收到调用请求时，会接受到传入SQL的ID和传入对象(可以是Map、JavaBean对象或者是基本数据类型)，Mybatis会根据SQL的ID找到对应的MappedStatement，然后根据传入参数对象对MappedStatement进行解析，解析后可以得到最终要执行的SQL语句和参数
    3. SQL执行：将最终得到的SQL语句和参数传入数据库中执行，得到数据库返回的执行结果
    4. 结果映射：将操作数据库的结果按照映射的配置进行转换，可以转换成HashMap，JavaBean或者基本数据类型，并将最终结果返回
<center>
![Mybatis框架架构](http://7xqvf0.com1.z0.glb.clouddn.com/%E5%8A%9F%E8%83%BD%E6%9E%B6%E6%9E%84.png)
Panda
</center>
