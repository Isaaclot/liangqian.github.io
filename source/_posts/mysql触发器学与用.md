---
title: mysql触发器学与用
date: 2016-07-25 21:38:58
tags: Mysql
description: 触发器可以用于记录对数据库的操作，它是一种与表操作有关的数据库对象，当触发器所在表上出现某个触发事件以及触发事件时，将调用该对象。即表的操作事件触发表上的触发器的执行
---
### 简述：
+ Mysql在5.0.2版本以上开始支持触发器，触发器是有某些带有命令的的时间来出发某些操作，这些事件操作包括 insert delete update...
+ 触发器可以用于记录对数据库的操作，它是一种与表操作有关的数据库对象，当触发器所在表上出现某个触发事件以及触发事件时，将调用该对象。即表的操作事件触发表上的触发器的执行

### 创建触发器
+ 语法：
```
CRATE TRIGGER TRIGGER_NAME 
TRIGGER_TIME
TRIGGER_EVENT ON TRIGGER_TABLE
FOR EACH ROW
TRIGGER_STATEMEMTS
```
+ 解析：
	+ TRIGGER_NAME : 表示触发器名称，用户自定义
	+ TRIGGER_TIME :  标识触发时机，取值为：before 、 after 
	+ TRIGGER_EVENT : 标识触发事件，取值为：INSERT , UPDATE ,DELETE
	+ TRIGGER_TABLE ：标识建立触发器的表名，也就是触发器的表对象
	+ TRIGGER_STATEMENT：触发器程序体，可以是一条语句，或如果是多条语句，用`begin [stmt_list] end;`包含，但是每条语句之间用分好分隔
	+ 根据触发时机与触发事件的组合，可以看出，最多可以建立6种触发器(注：一个表上不能简历相同的触发器)
+ 对于`begin...end`，由于中间使用了分号(mysql中标记执行完语句的符号)而导致begin找不到与之匹配的end，因此引入了`DELIMITER`命令，作为定界分隔符；`end$ DELIMITER ;`

### 一个完整的触发器示例
```
DELIMITER $
create trigger tri_stuInsert after insert
on student for each row
begin
declare c int;
set c = (select stuCount from class where classID=new.classID);
update class set stuCount = c + 1 where classID = new.classID;
end $
DELIMITER;
```
+ mysql用使用DECLARE 来定义局部变量，并且该遍历只能在BEGIN...END之间使用，并且只能放在期间复合语句的开头
```
格式：DECLARE var_name[,...] type [DEFAULT value]
```
+ new与old：
	+ INSERT型触发器 ：NEW表示将要(BEFORE)或已经(AFTER)插入的新数据
	+ UPDATE型触发器：OLD用来表示将要或者已经被修改的原数据，NEW表示将要或者已经被修改的新数据
	+ DELETE型触发器：OLD用来表示将要或已经被删除的数据
	+ 并且，OLD是只读的，而NEW可以在触发器中使用set赋值，避免二次触发触发器，造成循环调用

### 查看触发器
+ 语法：可以在后面指定数据库名来查看触发器
```
show triggers [from scheme_name];
```

### 删除触发器
+ 语法：
```
drop trigger [if exists] trigger_name;
```

### 触发器执行顺序
+ 我们建立的数据库一般都是 InnoDB 数据库，其上建立的表是事务性表，也就是事务安全的。这时，若SQL语句或触发器执行失败，MySQL 会回滚事务，有：
1. 如果 BEFORE 触发器执行失败，SQL 无法正确执行
2. SQL 执行失败时，AFTER 型触发器不会触发
3. AFTER 类型的触发器执行失败，SQL 会回滚