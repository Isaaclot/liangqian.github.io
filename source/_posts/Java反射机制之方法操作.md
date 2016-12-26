---
title: Java反射机制之方法操作
date: 2016-10-8 16:56:57
tags: [Java,动态代理]
description: 使用 Java 反射你可以在运行期检查一个方法的信息以及在运行期调用这个方法，通过使用 java.lang.reflect.Method 类就可以实现上述功能
---
## Java反射机制之方法操作

#### 获取方法对象
+ `Method[] methods = aClass.getMethods();`返回的 Method 对象数组包含了指定类中声明为公有的(public)的所有变量集合
+ `Method method = aClass.getMethod("doSomething", new Class[]{String.class});`返回方法名为doSomeThing，参数为String的方法对象
+ `Method method = aClass.getMethod("doSomething", null);`返回无参方法
+ `Method privateStringMethod = PrivateObject.class.
        getDeclaredMethod("getPrivateString", null);`返回私有

#### 方法参数以及返回类型
+ `Class[] parameterTypes = method.getParameterTypes();`获取指定方法的方法参数
+ `Class returnType = method.getReturnType();`获取指定方法的返回类型

#### 通过Method对象调用方法
```
Class myClass = Result.class;
        Result result = new Result();
        Method method = myClass.getMethod("getName");
        Method method1set = myClass.getMethod("setName",String.class);
        method1set.invoke(result,"TENYUAN");
        System.out.println(method.invoke(result));
```
