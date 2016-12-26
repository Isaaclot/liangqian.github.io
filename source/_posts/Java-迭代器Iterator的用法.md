---
title: Java 迭代器Iterator的用法
date: 2016-07-28 00:12:22
tags: Java
description: 迭代器是一种设计模式，它是一个对象，它可以遍历并选择序列中的对象，而开发人员不需要了解该序列的底层结构;迭代器通常被称为“轻量级”对象，因为创建它的代价小.
---
### 迭代器(Iterator)概述
+ Iterator是作为一个接口存在的，它定义了迭代器所具有的功能
+ 迭代器是一种设计模式，它是一个对象，它可以遍历并选择序列中的对象，而开发人员不需要了解该序列的底层结构;迭代器通常被称为“轻量级”对象，因为创建它的代价小.

### 迭代器的好处
1. 迭代器可以提供统一的迭代方式。
2. 迭代器也可以在对客户端透明的情况下，提供各种不同的迭代方式。
3. 迭代器提供一种[快速失败机制](http://wiki.jikexueyuan.com/project/java-enhancement/java-thirtyfour.html)，防止多线程下迭代的不安全操作。

### 功能方法
+ 示例:
```
Iterator<String> iter = list.iterator();
		while(iter.hasNext())
		{
			iter.next();
			//System.out.println(iter.next());
		}
```
+ Java中的迭代器功能比较简单，并且只能单向移动;
+ 方法:`iterator();`要求容器返回一个Iterator;第一次调用Iterator的next()方法时，它返回序列的第一个元素
+ `next();`获得序列中的下一个元素
+ `hasNext();`检查序列中是否还有元素
+ `remove();`将迭代器新返回的元素删除

### 知识扩充(遍历List集合的方法)
+ foreache 遍历：
```
for(String tmp:list){
			System.out.println(tmp);
		}
```
+ for 循环遍历：
```
for(int i = 0; i < list.size(); i++){
			list.get(i);
			System.out.println(list.get(i));
		}
```
+ Iterator遍历(例子上面已经给出)
+ 其中，对于同一个List集合，统计每个遍历方式时间消耗如下：
```
List first visit method(foreache):
Run Time:170(ms)
List second visit method(for):
Run Time:10(ms)
List Third visit method(Iterator):
Run Time:34(ms)
```
+ 显而易见；迭代器遍历在时间上的优势很大，代码简洁
+ 同时，与for/foreach 相比，Iterator使用相同方式去遍历集合中元素，而不用考虑集合类的内部实现(只要它实现了 java.lang.Iterable 接口)
