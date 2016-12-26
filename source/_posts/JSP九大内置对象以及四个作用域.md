---
title: JSP九大内置对象、四个作用域以及六大基本动作
date: 2016-07-29 21:27:33
tags: [Java,Servlet,Web]
description: 内置对象(又叫隐含对象，有9个内置对象)不需要预先声明就可以在脚本代码和表达式中随意使用
---
## 内置对象
1. request：javax.servlet.ServletRequest的子类型，此对象封装了由WEB浏览器或其它客户端生成地HTTP请求的细节（参数，属性，头标和数据）
	+ 常用方法：getParameter、getParameterNames 和getParameterValues 通过调用这几个方法来获取请求对象中所包含的参数的值
2. response： javax.servlet.ServletResponse的子类型，此对象封装了返回到HTTP客户端的输出，向页面作者提供设置响应头标和状态码的方式。经常用来设置HTTP标题，添加cookie，设置响应内容的类型和状态，发送HTTP重定向和编码URL
3. out: javax.servlet.jsp.JspWriter类型，代表输出流的对象
	+ 常用的方法除了pirnt和println之外，还包括clear、clearBuffer、flush、getBufferSize和getRemaining，这是因为“out” 对象内部包含了一个缓冲区，所以需要一些对缓冲区进行操作的方法
4. session: javax.servlet.http.HttpSession类型，主要用于跟踪对话;HttpSession是一个类似哈希表的与单一WEB浏览器会话相关的对象，它存在于HTTP请求之间，可以存储任
何类型的命名对象;
	+ “session” 对象建立在cookie的基础上，所以使用时应注意判断一下客户端是否打开了cookie。常用的方法包括getId、 getValue、 getValueNames和putValue等
5. pageContext: javax.servlet.jsp.PageContext（抽象类）类型;此对象提供所有四个作用域层次的属性查询和修改能力，它也提供了转发请求到其它资源和包含其他资源的方法 ,该对象的方法都是抽象方法，代表的是当前页面运行的一些属性
	+ 常用方法：findAttribute、getAttribute、getAttributesScope 和getAttributeNamesInScope
	+ 一般情况下pageContext对象用到得也不是很多，只有在项目所面临的情况比较复杂的情况下，才会利用到页面属性来辅助处理。
6. application：javax.servlet.ServletContext类型，servlet的环境通过调用getServletConfig().getContext()方法获得;它提供了关于服务器版本，应用级初始化参数和应用内资源绝对路径，注册信息的方式得并设置会话属性
	+ 常用的方法有getMimeType和getRealPath等
7. config：javax.servlet.ServletConfig类型，（页面执行期）
	+ 常用的方法有getInitParameter和getInitParameterNames，以获得Servlet初始化时的参数
8. exception: java.lang.Throwable,通过JSP错误页面中一个catch块已经益出但没有捕获的java.lang.Throwable的任意实例，传向了errorPage的URI。作用域为page（页面执行期）。注意exception只有在page指令中具有属性isErrorPage="true"时才有效。
9. page:java.lang.Object类型，指向页面自身的方式

## 内置对象与作用域对应关系
|对象|对象名称|所属类|作用域|
|---|---|---|---|
|request|请求对象|javax.servlet.ServletRequest|Request|
|response|相应对象|javax.servlet.SrvletResponse|Page|
|pageContext|页面上下文|javax.servlet.jsp.PageContext|Page|
|session|会话对象|javax.servlet.http.HttpSession|Session|
|application|应用程序对象|javax.servlet.ServletContext|Application|
|out|输出对象|javax.servlet.jsp.JspWriter|Page|
|config|配置对象|javax.servlet.ServletConfig|Page|
|page|页面对象|javax.lang.Object|Page|
|exception|例外对象|javax.lang.Throwable|page|

## 作用域
|作用域|有效范围|
|---|---|
|pageContext|有效范围只是在当前jsp页面|
|request|有效范围只是在当前请求周期|
|session|当前会话(用户打开浏览器到用户关闭浏览器之间的过程),也就是说，只要用户不关浏览器，服务器就有办法知道这些请求是一个人发起的,整个过程被称为一个会话,而放到会话中的变量，就可以在当前会话的所有请求里使用|
|application|整个应用(只要服务器不重启，都有效);与上述三个不同的是，application里的变量可以被所有用户共用|

## 六种基本动作
+ `<jsp:include page="" />`: 在页面请求中包含一个文件
+  `<jsp:useBean id="" class="" scope="application page request session" />`:寻找或者实例化一个javaBean
+ `<jsp:setProperty name="" property="" value="" />`:设置javaBean的属性，通过反射调用方法
+ `<jsp:getProperty name="" property=""/>`:取得某个javaBean的属性
+ `<jsp:forward page=""/>`:把请求转到一个新的页面
+ `<jsp:plugin>`:插入Applet程序的代码
+ `<jsp:param name="" value="" />`:用于传参数，和forward一起使用
