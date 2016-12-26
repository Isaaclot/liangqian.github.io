---
title: Java反射机制之动态代理
date: 2016-10-10 16:54:22
tags: [Java,动态代理]
description: 给某个对象提供一个代理对象，并由代理对象控制对于原对象的访问，即客户不直接操控原对象，而是通过代理对象间接地操控原对象
---
## Java反射机制之动态代理

#### 代理模式概述
+ 定义：给某个对象提供一个代理对象，并由代理对象控制对于原对象的访问，即客户不直接操控原对象，而是通过代理对象间接地操控原对象，如图：
[代理模式(以静态代理为例)](http://7xqvf0.com1.z0.glb.clouddn.com/%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F.gif)
+ 上图中：
	+ RealSubject是原对象
	+ Subject是委托对象与代理对象都要实现的接口
	+ `request()` 是代理对象与委托对象都拥有的方法
+ 代理模式分类：
	+ 静态代理：代理类是在编译时就实现好的.也就是说 Java 编译完成后代理类是一个实际的 class 文件
	+ 动态代理：代理类是在运行时生成的。也就是说 Java 编译完之后并没有实际的 class 文件，而是在运行时动态生成的类字节码，并加载到JVM中

#### 动态代理概述
+ Java反射机制你可以在运行期动态的创建接口的实现
+ 动态的代理的用途十分广泛，比如数据库连接和事物管理（transaction management）还有单元测试时用到的动态 mock 对象以及 AOP 中的方法拦截功能等等都使用到了动态代理

##### 实现步骤
+ 定义一个委托类和公共接口
+ 自己定义一个类(调用处理器,即是实现`InvacationHandler`接口)，指定运行时将生成的代理类需要完成的具体任务
+ 动态生成代理对象，指定委托对象实现的一系列接口调用处理器的实例
+ 通过代理对象调用方法

##### Java API提供实现动态代理的类或接口
1. Proxy(java.lang.reflect.Proxy):这是生成代理类的主类，通过 Proxy 类生成的代理类都继承了 Proxy 类(`DynamicProxyClass extends Proxy`)
	2. 创建代理对象的方法：`static Object newProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler h)`,其中，以一个参数是类加载器，第二个参数是需要代理类实现的接口，第三个参数是调用处理器类实例
	3. `InvocationHandler getInvocationHandler(Object proxy)`: 获得代理对象对应的调用处理器对象
	4. `Class getProxyClass(ClassLoader loader, Class[] interfaces)`: 根据类加载器和实现的接口获得代理类

2. InvocationHandler(`java.lang.reflect.InvocationHandler`):动态生成的代理类需要完成的具体内容需要自己定义一个类，而这个类必须实现 InvocationHandler 接口

#### 动态代理Demo Codes Showed
```
package xyz.royleo.Proxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

/**
 * Created by RoyLeo on 2016/10/14.
 */
public class DynamicProxyDemo {
    public static void main(String[] args) {
        RealSub realSub = new RealSub();
        ProxySub handler = new ProxySub(realSub);
        Sub proxySubject = (Sub) java.lang.reflect.Proxy.newProxyInstance(RealSub.class.getClassLoader(),RealSub.class.getInterfaces(),handler);
        proxySubject.request();
    }
}

interface Sub{
    void request();
}

class RealSub implements Sub{
    @Override
    public void request() {
        System.out.println("RealSub-----Request");
    }
}

class ProxySub implements InvocationHandler{
    private Sub sub;
    public ProxySub(Sub sub) {
        this.sub = sub;
    }
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("=====Before======");
        Object result = method.invoke(sub, args);
        System.out.println("=====After=======");
        return result;
    }
}
```
