---
title: Java反射机制之变量操作
date: 2016-10-4 16:58:38
tags: [Java,动态代理]
description: 使用 Java 反射机制你可以运行期检查一个类的变量信息(成员变量)或者获取或者设置变量的值。通过使用 java.lang.reflect.Field 类就可以实现上述功能
---
## Java反射机制之变量操作

#### 获取Field对象
+ `Field[] methods = aClass.getFields();`返回Field 对象数组包含了指定类中声明为公有的(public)的所有变量集合
+ `Field field = aClass.getField("someField");`获得指定变量,如果没匹配到对应的变量，抛异常`NoSuchFieldException`
+ `Field[] fields = myClass.getDeclaredFields();`获取私有成员变量

#### 获取变量名称
+ `String fieldName = field.getName();`

#### 获取变量类型
+ `Object fieldType = field.getType();`

#### 获取或设置{get/set}变量值
+ 一旦获得了一个 Field 的引用，你就可以通过调用 Field.get()或 Field.set()方法，获取或者设置变量的值
+ 获取\设置普通成员变量的值
```
 Class myClass = Result.class;
        Result result = new Result();
        Field f = myClass.getField("omg");
        f.set(result,"appAPP");
        System.out.println(f.get(result));
```
+ 获取\设置私有变量的值：需要设置私有变量的访问权限`field.setAccessible(true)`
```
 Class myClass = Result.class;
        Result result = new Result();
        Field f = myClass.getDeclaredField("name");
        f.setAccessible(true);
        f.set(result,"appAPP");
        System.out.println(f.get(result));
```
