---
title: Java线程管理
date: 2016-09-30 21:59:18
tags: [Java,《Java 7并发编程实战手册》读书笔记]
description: 关于线程的创建、运行、中断等的一些操作的读书笔记，也好自己回头重温一下基础知识
---
## 线程管理

#### 线程的创建与运行
1. 线程创建：：
	2. 继承Thread类，并且覆盖run()方法
	3. 创建一个实现`Runnable`接口的类，并且覆盖`run()`方法，使用带参数的`Thread`构造器来创建`Thread`对象，这个参数就是实现`Runnable`接口的类的一个对象
4. 线程的运行：
	5. 线程对象调用`start()`方法，才会创建运行这个线程(线程创建，运行的时间点).

#### 线程信息的获取和设置
1. 线程的状态(status):`Thread.getStatus()`
2. 线程的唯一标识符(Id):`Thread.getId()`
3. 线程的名称(Name):`Thread.getName()`
4. 线程的优先级(Property):`Thread.getPrioity()`

#### 线程的中断
1. 线程中断方法：`Thread.interrupt()`调用时，Thread类里面标记线程是否中断的属性(interrupted)被标记为true
2. 检查线程是否中断：返回interrupted的属性值
	3. `Thread.interrupted()`
	4. `Thread.isInterrupted()`(能设置interrupt的属性为false)

#### 线程的休眠与恢复
+  `sleep(long timeouts)`:线程休眠指定时间，休眠期间，线程会释放CPU并且不在继续执行任务

#### 等待线程的终止
+ `Thread1.join()`:等待Thread1线程执行完之后才运行其他线程或者任务(适用于一些初始化任务等...)

#### 守护线程的创建与运行
1. 守护线程：优先级很低，当程序中没有其他的线程运行的时候，才运行守护线程，然后JVM就结束程序
2. 创建：`setDaemon(true)`创建守护线程(守护线程必须先在`start()`方法被调用之前设置)

#### 线程中不可控异常处理
1. 非运行时异常(Checked Exception)：这种异常必须在方法声明用throws语句来指定，或者在方法内捕获(例如：IOException,ClassNotFoundException..)
2. 运行时异常(Unchecked Exception)：不必在方法声明指定，也不必捕获(例如：NumberFormatException)
3. `run()`方法不支持throws语句

#### 线程局部变量
+ 避免一个线程中改变了一个属性，所有线程都会被这个改变而影响
+ 用法：通过声明一个`ThreadLocal<T>`对象，这个对象是在`initialValue()`方法中隐式实现的.
```
 private static ThreadLocal<Date> startDate = new ThreadLocal<Date>(){
        @Override
        protected Date initialValue() {
            return new Date();
        }
    };
```

#### 线程的分组
+ 把一个组的线程当作一个单一的单元，对组内线程对象进行访问并操作它们
+ 创建：通过创建线程数组，创建线程组，`ThreadGroup.enumerate(Thread[] threads)`获取线程组包含的线程列表

#### 通过工厂类创建线程
+ 创建：实现`ThreadFactory`接口，`@override newThread(Runnable r)`创建线程对象
