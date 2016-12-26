---
title: Spring-MVC请求处理工作流程
date: 2016-07-16 15:04:58
tags: Spring
---
## 前言
+ Spring Web MVC框架提供了model-view-controller 的体系结构，可以用来开发灵活的，借助spring的AOP特性，实现松耦合的Web应用程序组件；
+ MVC模式导致了应用程序不同方面的分离(输入业务、逻辑业务和UI逻辑)，同时提供了这些元素之间的松散耦合
	+ Model：封装应用程序数据，并且通常由POJO组成
	+ View: 主要用于呈现模型数据，并且由它生成客户端浏览器能够解析的HTML输出
	+ Controller：主要用于处理客户请求，并且构建合适的模型，并传递给视图呈现

## DispatcherServlet
+ Spring MVC框架中，DispatcherServlet用来处理所有的HTTP请求和响应
#### 工作流程如下：
![DispatcherServlet](http://7xqvf0.com1.z0.glb.clouddn.com/mvc1.png)
#### 当请求触发的时候...
+ 收到一个HTTP请求之后，DispatcherServlet根据HandlerMapping来选择并且调用适当的控制器
+ 控制器接受请求，并基于使用GET或者POST方法来调用适当的Service方法。Service方法将设置基于定义业务逻辑的数据模型，并返回视图名称到DispatcherServlet中
+ DispatcherServlet在ViewResolver的协助下，检索获取指定的视图
+ 当视图确定之后，DispatcherServlet把模型数据传递给视图，最后在浏览器中呈现
## 部分配置以及代码说明
+ 初始化DispatcherServlet,导入Servlet-Name的应用配置文件`{servlet-name}-servlet.xml`
```
<servlet>
      <servlet-name>HelloWeb</servlet-name>
      <servlet-class>
         org.springframework.web.servlet.DispatcherServlet
      </servlet-class>
      <load-on-startup>1</load-on-startup>
   </servlet>
```
+ 定义处理的URL类型(对应servlet-name)
```
<servlet-mapping>
      <servlet-name>HelloWeb</servlet-name>
      <url-pattern>*.jsp</url-pattern>
   </servlet-mapping>
```
+ 定义视图解决方案
```
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
      <property name="prefix" value="/WEB-INF/jsp/" />
      <property name="suffix" value=".jsp" />	//收到DispatcherServlet返回的视图名称之后，显示`[viewName].jsp`文件
   </bean>
```
+ 定义控制器
```
@Controller
@RequestMapping("/hello")  //url路径
public class HelloController{
   @RequestMapping(method = RequestMethod.GET) 		//使用GET方法
   public String printHello(ModelMap model) {
      model.addAttribute("message", "Hello Spring MVC Framework!"); 	//绑定Message属性的值，并传递给DispatcherServlet
      return "viewName";  		//返回给DispatcherServlet的视图名称
   }
}
```
+ 视图,下例是一个jsp文件的部分代码，message是在控制器内部设置的属性
```
<html>
   <head>
   <title>Hello Spring MVC</title>
   </head>
   <body>
   <h2>${message}</h2>
   </body>
</html>
```
## 拓展
+ 目前流行的Restful风格的设计，可以借助Spring-MVC实现，更好的实现前后端分离，控制器之间通过DispatcherServlet，给View页面传递一组 JSON 或者是 XML 等形式的数据，view接受到数据之后，通过Angular JS /reaact等其他一些Javascript的一些单页框架，处理后端传过来的数据，并显示出来.
