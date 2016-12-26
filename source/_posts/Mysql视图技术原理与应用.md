---
title: Mysql视图技术原理与应用
date: 2016-07-26 21:21:17
tags: Mysql
description: 视图是存储在数据库中的查询的SQL 语句，它主要出于两种原因：安全原因， 视图可以隐藏一些数据，如：社会保险基金表，可以用视图只显示姓名，地址，而不显示社会保险号和工资数等，另一原因是可使复杂的查询易于理解和使用。这个视图就像一个“窗口”，从中只能看到你想看的数据列
---
### 视图概述
+ 定义：视图是一个虚拟表，其内容由查询定义，视图表内存在行列数据，但是不在数据库中以存储的数据值集形式存在，行和列数据来自由定义视图的查询所引用的表，并且在引用视图时动态生成.
+ 更新过程：当查询视图时，数据库从相应的引用表中引入数据到视图表
+ 视图技术优点：
	+ 视图能简化用户操作：视图机制使用户可以将注意力集中在所关心的数据上，使数据库看起来结构简单，清晰，并简化用户的查询操作
	+ 视图使用户能以多种身份看待数据：灵活地实现多种不同类型的用户共享同一个数据库
	+ 视图对重构数据库提供了不同程度上的逻辑独立性：重构时(数据库逻辑改变)，新增关系表或者对原有关系增加新字段，不会影响应用程序所需的数据查询形式
	+ 适当的利用视图可以更清晰地表达查询

### 使用案例:
#### 语法：
+ `ALGORITHM`:定义查询算法
	+ `MERGE`:将查询视图的语句与视图的定义语句合并处理
	+  `TEMPTABLE`:视图查询的结果保存到临时表，而后在该临时表基础上执行查询视图的语句
	+  `UNDEFINED` : 由Mysql选择使用的算法，一般首选MERGE，因为MERGE更有效率，而且TEMPTABLE不支持更新
+  WITH[CASCADED|LOCAL] CHECK OPTION 解析
	+  LOCAL参数表示更新视图时只要满足该视图本身定义的条件即可
	+  CASCADED参数表示更新视图时需要满足所有相关视图和表的条件;没有指明时，该参数为默认值
```
CREATE [OR REPLACE] [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}] VIEW [db_name.]view_name [(column_list)] AS select_statement [WITH [CASCADED | LOCAL] CHECK OPTION]
```
#### 创建视图：
+ 在单表上创建：`create view vName(id,sex,age) as select id,sex,age from student;`
+ 在多表上创建：`CREATE VIEW VName(ID, NAME, SEX, AGE,DEPARTMENT,POS,EXPERENCE) AS SELECT a.ID, a.NAME, a.SEX, a.AGE,a.DEPARTMENT,b.POS,b.EXPERENCE FROM learning.t_employee a,learning.t_employee_detail b WHERE a.ID=b.ID;`
+ 查看视图：`DESCRIBE vName;`或`SHOW TABLE STATUS;`或`SHOW CREATE VIEW VnAME;`
+ 修改视图：
	+ ` create or replace` 指令：`CREATE OR REPLACE VIEW V_VIEW1(ID, NAME, SEX) AS SELECT ID, NAME, SEX  FROM learning.t_employee; `
	+ `ALERT`指令: `ALTER VIEW  V_VIEW1(ID, NAME) AS SELECT ID, NAME  FROM learning.t_employee;`
+ 更新视图：主要通过(insert,update,delete)操作表中数据；由于视图上的数据来自于基本表，更新视图虚拟表上的数据都会转换到基本表更新.
	+ 更新视图时，只能更新权限范围内的数据，超出范围，不能更新
	+ 指令实例：`update vName set project="psychonologic" where id = 2;`
	+ 不可更新的视图：由于虚拟表对应实体表中的数据，所以不是所有的视图表都能更新；如果视图含有下列结构中的任何一种，则不能更新：
		+ 聚合函数(SUM(),MIN(),MAX(),COUNT()...)
		+ DISTINCT
		+ GROUP BY
		+ HAVING
		+ UNION/UNION ALL
		+ 位于选择序列中的子查询
		+ Jion
		+ FROM字句中的不可更新视图
		+ WHERE子句的子查询，引用FROM字句的表
		+ 仅引用文字值
		+ ALGORITHM = TEMPTABLE
	+ 对于更新操作的提醒：视图虽然可以更新数据，但是有很多限制，一般情况下，最好将视图作为查询数据的虚拟表，而不要通过视图更新数据. 对于一些结构更加复杂的数据库表，最好在实体表中更新数据