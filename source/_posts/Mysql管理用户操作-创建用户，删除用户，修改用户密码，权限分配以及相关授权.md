---
title: Mysql管理用户操作(创建用户，删除用户，修改用户密码，权限分配以及相关授权)
date: 2016-06-08 15:30:11
tags: Mysql
---
#### 创建mysql用户
+ 创建用户设定密码：
  `create user 'username' identified by 'password';`
+ 创建用户，密码，并绑定登陆IP(ip:%表示所有IP，也可以限制为网段[231.23.2.%])
  `create user 'username'@'ip' identified by 'password'`
+ 使操作生效：`flush privileges`

#### 为用户授权
+ 将用户所有的权限绑定到某个数据库：
  `grant all privileges on basename(*).tablename(*) to username@localhost`
+ 或者只授予用户指定的权限：`insert` `delete` `update` `select`...
  `grant insert,select,update,delete on *.* to user@localhost`

#### 撤销用户权限
+ 格式：`revoke 权限 on 库名.表名 from 用户名@用户地址;`

#### 删除用户
+ 直接删除用户：`drop user`(将用户信息全部删除)
```
>drop user test1@'%';
>flush privileges;   
```
+ 删除用户：`delete from user`(只会清除对应user创建的表)
```
>use mysql;
>delete from user where user=test1 and host='%';
>flush privileges;
```
#### 修改指定用户的密码(也就是更新mysql库下user的表)
```
>>update mysql.user set password=password('新密码') where  user="test2" and host="%";
>>flush privilieges;
```
#### 远程登陆
```
>mysql -h 'ip' -u 'username' -p 'ip'  ##个人习惯enter之后输密文password
```
#### tips
+ mysql库下面有几个表，研究清楚了，也就更加了解了mysql的基本操作了
+ 还有就是，新版本的mysql数据库自动为你创建了几个常用的库跟表，比如'country'等....(然而我很少用)
