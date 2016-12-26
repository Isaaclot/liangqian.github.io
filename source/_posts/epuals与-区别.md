---
title: "epuals与'=='区别"
date: 2016-09-12 00:50:40
tags: Java
---

### Java数据类型分类
+ 基本数据类型(原始类型)：byte short char int long float double boolean
	+ 他们的比较，使用`==`,比较的是他们的值
+ 复合数据类型(类定义)
	+ 他们通过`==`比较的时候，比较的是他们存放的内存地址，所以，除非是同一个new出来的对象，他们的比较结果是true，否则，比较结果是false
	+ Java所有的对象都是继承自IObject这个基类，在Object基类中定义了一个`equals()`的方法,这个对象的初始行为是比较对象的内存地址，但是有一些类库中，将这个方法覆盖掉了，如String，Integer，Date在这些类中equals()有着自身的实现，不再是比较类在堆内存中的存放地址。
	+ 而对于，那些木有覆盖`equals();`方法的的复合数据类型，他们之间的比较还是比较存放在内存中的地址值，与使用`==`比较结果相同


### `==`操作
+ “==”或等号操作在Java编程语言中是一个二元操作符，用于比较原生类型和对象。
+ 就原生类型如boolean、int、float来说，使用“==”来比较两者，这个很好掌握。但是在比较对象的时候，就会与equals()造成困惑。“==”对比两个对象基于内存引用，如果两个对象的引用完全相同（指向同一个对象）时，“==”操作将返回true，否则返回false。


### `equals();`方法
+ equals()方法定义在Object类里面，根据具体的业务逻辑来定义该方法，用于检查两个对象的相等性。
+ 例如：两个Employees被认为是相等的如果他们有相同的empId的话，你可以在你自己的domain对象中重写equals方法用于比较哪两个对象相等。
+ equals与hashcode是有契约的（无论什么时候你重写了equals方法，你同样要重写hashcode()方法），默认的equals方法实现是与“==”操作一样的，基于业务需求重写equals方法是最好的实践之一，同样equals与compareTo保持一致也不足为奇，以至于存储对象在Treemap或treeset集合中时，将使用compareTo方法检查相等性，行为是一致的。

### `==`与`equals();`区别
+ ==常用于比较原生类型，而equals()方法用于检查对象的相等性。
+ 另一个不同的点是：如果==和equals()用于比较对象，当两个引用地址相同，==返回true。而equals()可以返回true或者false主要取决于重写实现。最常见的一个例子，字符串的比较，不同情况==和equals()返回不同的结果。

### 献上代码：
```
/**
 * Created by RoyLeo on 2016/9/12.
 */
public class main {
    public static void main(String[] args){
        String t1 = "你好啊";
        String t2 = "你好啊";
        String tt = t2;
        String t3 = new String("你好啊");
        String t4 = new String("你好啊");
        String ttt = t3;
        System.out.println(ttt==t4);             //false
        System.out.println(ttt==t3);            //true
        System.out.println(t1==t2);             //true
        System.out.println(t1==t3);             //false
        System.out.println(t3==t4);             //false
        System.out.println(t1.equals(t2));      //true
        System.out.println(t1.equals(t3));      //true
        System.out.println(t3.equals(t4));      //true
    }
}

```
