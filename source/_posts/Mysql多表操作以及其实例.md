---
title: "'Mysql多表操作以及其实例'"
date: 2016-9-17 21:46:28
tags: Mysql
---
## Mysql多表操作及其实例

#### 数据表：
+ 学生表
```
+---------+-------------+------+-----+---------+----------------+
| Field   | Type        | Null | Key | Default | Extra          |
+---------+-------------+------+-----+---------+----------------+
| stuId   | int(11)     | NO   | PRI | NULL    | auto_increment |
| classId | int(11)     | YES  |     | NULL    |                |
| stuName | varchar(10) | YES  |     | NULL    |                |
+---------+-------------+------+-----+---------+----------------+
3 rows in set (0.01 sec)
```
+ 成绩表
```
mysql> desc score;
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| classId | int(11)     | YES  |     | NULL    |       |
| stuId   | int(11)     | YES  |     | NULL    |       |
| course  | varchar(10) | YES  |     | NULL    |       |
| score   | int(11)     | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```
+ 班级表
```
mysql> desc class;
+-----------+-------------+------+-----+---------+----------------+
| Field     | Type        | Null | Key | Default | Extra          |
+-----------+-------------+------+-----+---------+----------------+
| classId   | int(11)     | NO   | PRI | NULL    | auto_increment |
| className | varchar(20) | YES  |     | NULL    |                |
+-----------+-------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)
```

####  数据库操作用例
1. 查询“语文”课程比“数学”课程成绩高的所有学生的学号
```
select * from (select stuId,score from score where course='语文' ) a,
(select stuId,score from score where course='数学') b,(select * from student) c
 where a.stuId=b.stuId and a.score>b.score and a.stuId=c.stuId;
```
2. 查询平均成绩大于60分的同学的学号和平均成绩
```
select stuId,avg(score) from score group by stuId having avg(score)>60;
```
3. 查询所有同学的学号、姓名、选课数和平均成绩
```
select student.stuId,student.stuName as '姓名',count(score.classId),sum(score.score) as '所选课程总分'
from student left join score on student.stuId=score.stuId group by student.stuId
```
4. 查询所有姓'雷'的学生个数
```
select count(student.stuName) from student where stuName like '雷%'
```
5. 查询所有课程成绩小于60分的同学的学号，姓名
```
select student.stuId,student.stuName from student where stuId not in
 (select score.stuId from score where score.score>85)
```
6. 查询各科的最高分的学生的学号，姓名等信息
```
SELECT *,max(score) as max FROM info.score,info.student
where student.stuId=score.stuId group by score.course;
```
7. 查询各班的第一名学生的信息，按照班级排序
```
SELECT *,max(score) as max FROM info.score,info.student ,info.class
where student.stuId=score.stuId and student.classId=class.classId group by class.classId order by class.className;
```
