---
title: Java 实现TCP/IP Socket通信
date: 2016-07-24 00:30:07
tags: Java
description: 整理了之前计算机网络课程实验跟笔记，在Java上实现TCP/IP 的Socket通信，其实这也是算熟悉了，一年前就写了一个.NET下的Socket，只是当时还不懂其中原理(一脸懵逼)，现在就整理了个博文出来，Github仓库有代码
---
##### Java提供的网络功能有四大类
+ InetAddress; 用于标识网络上的硬件资源，比如MAC地址等．．．
+ URL：统一资源定位符 通过URL可以直接读取或写入网络上的数据
+ Sockets：使用TCP协议实现网络通信的Socket的相关类
+ Datagram：使用UDP协议，将数据保存在数据报中，通过网络进行通信
##### InetAddress类：表示互联网协议(IP)地址
+ 通过静态方法获取实例：(然后就可以根据jdk文档，使用相关方法)
```
InetAddress address = InetAddress.getLocalHost();//或者
InetAddress address1 = InetAddress.getByName("10.2.18.87");
```
##### Socket实现TCP通信(基于数据流套接字)
+ 建立连接流程： 服务器处于监听状态，等待并接受客户端创建的socket向服务器发送请求，服务器接收请求之后，创建连接Socket进行通信
+ 开始通信：通过相关的输入流(InputStream)、输出流(OutputStream)，发送接收数据
+ 结束通信：关闭客户端和服务端的socket，释放资源
+ 实现步骤：
	1. 创建ServerSocket和Socket
	2. 打开连接到Socket的输入/输出流
	3. 按照协议对Socket进行读写操作
	4. 通信完毕，关闭输出流，关闭Socket
+  服务端实现步骤：
	1.	创建ServerSocket对象，绑定监听端口
	2.	通过accept()方法绑定客户端请求
	3.	建立连接后，通过输入流读取客户端发送的请求信息
	4.	通过输出流向客户端发送响应信息
	5.	关闭相关资源
+ 客户端实现步骤：
	1.	创建Socket对象，指明需要连接的服务器的地址与端口号
	2.	连接创建后，通过输出流向服务器发送请求信息
	3.	关闭相关资源
+  主要方法：getInputStream(), getOutPutStream();实现发送/接受数据 
+  多线程实现服务器与多客户端之间通信
	+  基本步骤：
		1. 在服务器端创建ServerSock，循环调用accept()等待客户端连接
		2. 客户端创建一个Socket并请求和服务端连接
		3. 服务端就收客户端请求，创建socket与该客户简历专线连接
		4. 建立链接的两个Socket在一个单独的线程上对话
		5. 服务器机箱等待新的连接
##### UDP Socket编程
+ 使用的类：DatagramPacket类，DatagramSocket类；可以在jdk文档里面了解其中方法的用法
+ 服务端实现步骤：
	1. 创建DatagramSocket，指定端口号
	2. 创建DatagramPacket，用于接收客户端数据
	3. 接受客户端发送的数据
	4. 读取数据
+ 客户端实现步骤
	1. 定义发送信息
	2. 创建DatagramPacket，包含要发送的数据信息
	3. 创建DatagramSocket
	4. 发送数据
+ 同样，也可以通过像tcp Socket通信那样完善，添加多线程监控客户端请求并相应;
+ 备注：程序代码都在Github 仓库：[enter link description here](https://github.com/liangqian/socketDemo.git) 
