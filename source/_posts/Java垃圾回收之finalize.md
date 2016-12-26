---
title: Java垃圾回收之finalize()
date: 2016-05-08 00:07:13
tags:
---
## Java垃圾回收
+ Java垃圾回收器负责回收无用对象占据的内存资源
+ 但是，无论是“垃圾回收”或者“终结”，JVM都不保证会发生，因为JVM在没有面临内存耗尽的情况下，它是不会浪费时间去执行垃圾回收以恢复内存
+ JVM不会主动去发现一些无用特殊的内存区块，并清除它，所以引入`finalize()`方法

## `finalize()`用法
+ 对象可能不被垃圾回收
+ 垃圾回收并不等于“析构”
+ 垃圾回收只与内存有关

##### `finalize()`工作原理
+ 一旦垃圾回收器准备好释放对象占用的存储空间，将首先调用`finalize()`方法，并且在下一次垃圾回收动作发生时，才会真正回收对象占用的内存

#### 下面一个例子示范`finalize()`可能的使用方式(终结条件)
```
class Book{
  boolean checkedOut = false;
  Book(boolean checkOut){
    checkedOut = checkOut;
  }
  void chechIn(){
    checkedOut = false;
  }
  protected void finalize(){
    if(checkedOut){
      System.out.print("error:check out");
    }
  }
}
public class Terminal{
  public static void main(String[] args){
    Book novel = new Book(true);
    novel.chechIn();
    new Book(true);
    System.gc();
  }
}
/*
*output:
*error:check out
*/
```
###### 本例的终结条件：所有的Book对象在被当作垃圾回收前都应该被签入(chechIn);但是`main()`方法中，有一本书没被签入，所以如果没有`finalize()`来验证终结条件，那么这个缺陷很难被发现.
