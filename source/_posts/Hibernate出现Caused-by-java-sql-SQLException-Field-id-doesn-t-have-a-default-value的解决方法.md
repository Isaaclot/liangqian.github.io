---
title: "Hibernate出现Caused by: java.sql.SQLException: Field 'id' doesn't have a default value的解决方法"
date: 2016-06-05 23:53:46
tags: Hibernate
---

### 原因：设计Mysql的id的时候，id使用主键策略，但是没有设置自动生成造成
### 解决方案：
  + 取消主键策略(下下策)；
  + 更改结构语句：为主键id添加`auto_increment`属性；也就是更改表结构
