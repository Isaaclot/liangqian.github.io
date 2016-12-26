---
title: Spring中添加REST功能
date: 2016-07-31 20:39:26
tags: Spring
description: 近年来以信息为中心的表属性状态转移(Respresentational State Transfer REST)已经成为替代传统SOAP　Web服务的流行方案；而且Spring封装对REST的良好支撑
---
#### REST基本原理：
+ 通过简单的HTTP URL触发事件
+ REST是将资源最合适地从服务端转移到客户端(或者反之)
+ REST与RPC(远程过程调用)：REST面向资源，强调描述应用程序的事物和名词;RPC面向服务，关注于行为和行动
#### Spring如何支持REST？
+ 控制器可以处理所有的HTTP方法：GET、POST、DELETE、PUT...
+ @PathVariable 注解使控制器能够处理参数化的URL(带参数变量的URL)
+ Spring表单绑定JSP标签库的`<form:form>`标签以及新的HidenHttpMethodFilter，使得通过HTML转发提交的PUT，DELETE请求成为可能(即使mou'xie)
+ 使用String的视图和视图解析器，资源可以以各种形式表述，包括将数据模型表示为：XML、JSON、ATOM、RSS等
+ 使用新的ContentNegotiatingResolver来选择合适的客户端表述
+ 类似的，使用@ResponseBody注解以及HttpMethodConverter实现可以将传入的HTTP数据转化为传入控制器的处理方法的Java对象
+ RestTemplate简化客户端对REST资源的使用

#### RESTful URL
+ 格式：`http://localhost:8080/spitter/spittles/123`
	+ 协议：//域名：端口号/Servlet上下文路径/资源类型/特定的spittle
	+ REST　URL是用来标识资源
+ RESTful　URL是有层级结构的，每个层标识一种资源
+ 其中，URL还可以参数化，在控制器中获取该参数`@PathVariable("id") long id`

#### 执行REST动作
+ HTTP提供来操作资源的方法，主要是四个

|方法|描述|是否安全|
|---|---|---|
|GET|从服务器上检索资源信息，资源通过请求的URL来进行标识|是|
|POST|传送数据到服务器，数据会由监听该请求的URL处理器进行处理|否|
|PUT|按照请求的URＬ，防治资源到服务器指定位置|否|
|DELETE|将请求URL标识的资源从服务器上删除|否|

#### 表述资源
+ Spring提供两种形式，将资源的Java表述形式转换为发送给客户端的表述形式：
	+ 基于视图渲染进行协商
	+ HTTP消息转换器
+ 协商资源表述
bean配置:
``` <!-- 根据客户端的不同的请求决定不同的view进行响应, 如/blog/1.json/blog/1.xml-->
    <bean class="org.springframework.web.servlet.view.ContentNegotiatingViewResolver">
<!-- 设置为true以忽略对Accept Header的支持-->
         ##<property name="ignoreAcceptHeader" value="true"/>
 <!-- 在没有扩展名时即: "/user/1" 时的默认展现形式 -->
        <property name="defaultContentType" value="text/html"/>
<!-- 扩展名至mimeType的映射,即 /user.json => application/json -->
        <property name="mediaTypes">
            <map>
                <entry key="json" value="application/json" />
                <entry key="xml" value="application/xml" />
            </map>
        </property>
    </bean>```
1. 确定请求的媒体类型
	+ ViewResolver首先查找URL中的文件扩展名，将文件扩展名与mediatype的条目匹配，如果找到匹配项，则使用该媒体类型，并覆盖accpt头信息中的任何媒体类型
	+ 如果找不到URL中的扩展名，那么就使用Accept中的头信息的媒体类型
	+ 如果请求头中不包含Accept头信息，那么就使用defaultContentType属性中设置的媒体类型
2. 影响如何选择类型
	+ 将favorPathExtension属性设置为false，将会使得ContextNegotiatingViewResolver忽略URL路径扩展名
	+ 将JAF添加到类路径下将会使得ContextNegotiatingViewResolver除了使用mediaTypes属性中的条目外，在路径扩展名确定媒体类型时还会借助JAF
	+ 将favorParameter属性设置为true，并且请求中包含名为format参数，那么format参数的值将会mediatype属性来进行匹配，即使URL文件中没有文件扩展名也能匹配其中的媒体类型
	+ ignoreAcceptHeader属性设置为true，将会忽略Accept头信息
3. 查找视图
	+	ContextNegotiatingViewResolver循环所有保存的媒体类型，找到能与之匹配内容类型的视图，完成视图解析匹配
	+	如果ContextNegotiatingViewResolver没有找到合适的视图，那么它将返回null视图，或者如果useNotAcceptableStatusCode属性被设置为true，那么将返回带有http状态码406(Not Acceptable)的视图