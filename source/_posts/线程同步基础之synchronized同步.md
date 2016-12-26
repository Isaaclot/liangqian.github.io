---
title: 线程同步基础之synchronized同步
date: 2016-10-14 18:48:51
tags: [Java,《Java 7并发编程实战手册》读书笔记]
---
## 线程同步基础之synchronized同步
#### Java API提供的两种基本的同步机制
1. synchronized关键字机制
2. Lock接口及其实现机制

#### synchronized关键字概述
1. 对方法的声明定义
	+ 对于非静态方法，每个用synchronized关键字声明的方法都是一个临界区，对于同一个对象的临界区，再同一时间只有一个允许被访问
	+ 对于静态方法，
2. synchronized同步块
3. synchronized修饰一个类

#### 使用synchronized实现同步非静态方法
+ 使用synchronized修饰一个非静态方法是时候，修饰的是本方法，同一时间只能允许一个访问权限
```
    public synchronized void addAccount(double amount){
        double tmp = balance;
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        tmp += amount;
        balance = tmp;
    }
```

#### 使用synchronized实现同步静态方法
+ 我们知道静态方法是属于类的而不属于对象的。同样的，synchronized修饰的静态方法锁定的是这个类的所有对象
```
public synchronized static void method() {
   // todo
}
```

#### synchronized修饰代码块
+ 当两个并发线程访问同一个对象object中的这个synchronized(this)同步代码块时，一个时间内只能有一个线程得到执行。另一个线程必须等待当前线程执行完这个代码块以后才能执行该代码块
+ 当一个线程访问object的一个synchronized(this)同步代码块时，另一个线程仍然可以访问该object中的非synchronized(this)同步代码块
+ 当一个线程访问object的一个synchronized(this)同步代码块时，其他线程对object中所有其它synchronized(this)同步代码块的访问将被阻塞
```
public void method()  
{
    synchronized(表达式)  
     {  
     }   
}
```
+ 如果同步快里面的表达式是`this`关键字或者是`CLassName.class`关键字的话，作用的对象是这个类的所有对象

#### 使用非依赖属性实现同步
+ 线程同步控制过程中，创建一个无关属性，被多个线程共享使用，在同步操作中，同步对这个非依赖属性的访问

#### 在同步代码中使用条件
+ `wait()`:当一个线程调用`wait()`方法的时候，JVM将这个线程置入休眠，并且释放这个同步代码块的对象
+ `notify()`与`notifyAll()`：唤醒之前因为调用`wait()`方法的线程
+ 下面是生产者消费者模式里面的一个同步方法:当库存为零的时候，休眠次线程，不断的监听，库存是否继续为零
```
public synchronized void get(){
        while (storage.size()==0){
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Get : "+storage.size()+" "+((LinkedList<?>)storage).poll());
            notifyAll();
        }
    }
```
