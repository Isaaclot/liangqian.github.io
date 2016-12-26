---
title: Java反射机制之构造器操作
date: 2016-10-3 16:59:45
tags: [Java,动态代理]
description: 利用 Java 的反射机制你可以检查一个类的构造方法，并且可以在运行期创建一个对象。这些功能都是通过 java.lang.reflect.Constructor 这个类实现的
---
## 构造器反射

#### 获取Constructor对象
1. `Constructor[] constructors = aClass.getConstructors();`获取构造器数组
2.  `Constructor constructor =
  aClass.getConstructor(new Class[]{String.class});` 根据具体的构造器参数获取指定的构造器，如果没有匹配的方法，则抛异常`NoSuchMethodException`

#### 构造器参数
1. `Class[] parameterTypes = constructor.getParameterTypes();`获取构造器的方法参数信息

#### 利用构造器对象实例化一个类
+ constructor.newInstance()方法的方法参数是一个可变参数列表，但是当你调用构造方法的时候你必须提供精确的参数，即形参与实参必须一一对应
```
Constructor constructor = Result.class.getConstructor(new Class[]{String.class});
        Result result = (Result) constructor.newInstance("tettt");
        System.out.println(result.getName());
```
