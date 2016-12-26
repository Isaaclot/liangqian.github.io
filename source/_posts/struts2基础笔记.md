---
title: struts2基础笔记(1)
date: 2016-07-22 23:34:26
tags: struts2
---
### Sturts2访问Servlet API
+ 访问Servlet API的三种方式
	+ ActionContext：
```
 ActionContext actionContext=ActionContext.getContext();  
Map<String,Object> map=actionContext.getSession();  
map.put("hello","helloworld");  
```
	+ 实现***Aware接口：通过实现指定接口ServletContextAware，ServletRequestAware，ServletResponseAware，这种方式不推荐使用，与ServletAPI的耦合性大
```
public class LoginAction extends ActionSupport implements ServletRequestAware{ 
	HttpSession session=request.getSession();
	//something same as httpSetvlet
}
```
	+ ServletActionContext：ServletActionContext是面向action,特定于上下文的信息。继承于ActionContext
```
HttpServletRequest request=ServletActionContext.getRequest();  
    HttpSession session=request.getSession();  
    session.setAttribute("hello", "helloworld");
```
### Action的搜索顺序
1. 发生URL请求：`http://localhost:80/projectName/path1/path2/path3/test.action`
1.  首先会检索namespace为`/path1/path2/path3/`的package里面的test；如果找到则访问
	+ 否则，转向默认表空间[namespace=""]里面寻找该action，找到则访问
		+ 否则，转向默认表空间[namespace=""]寻找默认的action，找到则访问改action
			+ 如果默认空间中没有设置默认的action，则返回404....[not found]
1.  首先会检索namespace为`/path1/path2/`的package里面的test；如果找到则访问
	+ 否则，转向默认表空间[namespace=""]里面寻找该action，找到则访问
		+ 否则，转向默认表空间[namespace=""]寻找默认的action，找到则访问改action
			+ 如果默认空间中没有设置默认的action，则返回404....[not found]
1.  首先会检索namespace为`/path1/`的package里面的test；如果找到则访问
	+ 否则，转向默认表空间[namespace=""]里面寻找该action，找到则访问
		+ 否则，转向默认表空间[namespace=""]寻找默认的action，找到则访问改action
			+ 如果默认空间中没有设置默认的action，则返回404....[not found]
1.  如果仍然不存在这个package,就去默认的namaspace的package下面去找名字为test的action(默认的命名空间为空字符串""),
   如果还是找不到,页面提示找不到action,报404找不到内容的错误

### 动态方法调用
+ 概述：解决一个action对应多个请求的处理的问题，简化开发，避免Action太多
+ 方法一：指定Method属性
	+ 在配置文件中指定访问方法(method)：
``` <action name="hello" class="hellow" method="execute">
<result name="success">/result.jsp</result>
</action>
 ```
 
+ 方法二：感叹号方式
	+ 开启常量值配置：`<constant name="struts.enable.DynamicMethodInvocation" value="true"></constant>`
	+ 根据返回值在`<result>`里面用name指定视图：`<result name="result">/result.jsp</result>`
	+ 调用在action后缀之前添加感叹号和方法的方式访问：`http://localhost:8080/testStruts2/hello!result.action`
+ 方法三：通配符方式
	+ 在`<action>`配置里面name属性后面用`_*`的方式统配参数：第一个星号对应参数{1},第二个星号对应第二个参数{2}，可以统配包名，类名，方法名，或者部分名称的切割

```
 <action name="hello_*_*" class="{1}" method="execute">
<result name="success">/{2}.jsp</result>
  </action>
  ```
### 指定多个配置文件
+ 项目中action特别多，如果全在一个`struts.xml`里面，就很复杂
+ 可以通过<include>导入多个分割了的action配置文件模块：`<include file="hello.xml"></include>`
+ 也可以统一配置文件的编码：用`<constrant>`
### 默认action
+ 防止出现尴尬页面.....
+ 在`struts.xml`配置文件配置:
```
<default-action-ref name="hello"></default-action-ref>
<action name="hello"><result name="error">/error.jsp</result></action>
```
### struts2后缀：
+ 伪造页面，安全，防hacker
+ 改成html后缀：`<constant name="struts.action.extension" value="html"/>`
+ 取消后缀,只需消息value的值：`<constant name="struts.action.extension" value=""/>`
+ 也可以在struts.properties里面配置，还可以在里面配置多个参数
+ 可以在web.xml里面配置<init-param>来指定后缀
### 接收参数：
+ 方法一：使用Action属性接收：
	+ 在接受的类中创建与提交部分命名一致的属性，以及getter，setter；传过来 的时候就能接收
+ 方法二：使用Domain Model接收：
	+ 创建对象model来接收，为了将数据存入到指定的对象模型，在提交数据的时候，需要指定存入的已经实例化的对象的属性
```
 <label>用户名：</label><input type="text" name="us.username">
  <label>密  码：</label><input type="password" name="us.password">
```
+ 方法三：使用ModelDriven接收：
	+ 实现ModelDriven的接口；如果通过对象接受数据，必须实例化，并且，在提交数据的时候，不需要再指定属性传输的对象
```
  <label>用户名：</label><input type="text" name="username">
  <label>密  码：</label><input type="password" name="password">
  <label>书籍1：</label><input type="text" name="booklist[0]">
  <label>书籍2：</label><input type="text" name="booklist[1]">
  <label>书籍3：</label><input type="text" name="booklist[2]">  //user对象里面设置List集合接收
```
```
public class hwaction extends ActionSupport implements ModelDriven<user> {  //泛型定义需要赋值转换的类
    private user us = new user();  \\不需要getter setter...
    public String login(){
System.out.println(us.getUsername());
 System.out.println(us.getBooklist().get(2));
System.out.println(us.getBooklist().get(1));
System.out.println(us.getBooklist().get(0));
        return SUCCESS;}
     @Override
    public user getModel() { 
        return us; //返回需要转换的对象 }}  
```
+ 方法四：使用request方法接受 

###处理结果类型
+ 处理结果标签里面：type属性，默认(dispatcher)是定义jsp模板，还有其他的xstl,chain,redirect,stream,plaintext....
+ 五个系统内置属性：success，none，error，login，input
+ success：正确执行完成，返回响应的视图
+ none：正确执行，但是不返回视图
+ error：执行失败，返回到错误处理视图
+ login：由于用户没有登陆的原因没有正确执行，将返回登陆视图，要求用户进行登陆身份验证
+ input：Action的执行需要前端界面获取参数，input就是代表这个参数的输入界面
	+ 表单验证的出现类型转换错误的时候会跳转回去
	+ 在判断验证部分使用`this.addFieldError("username","用户名不为空")`，使用`return input;`跳转，并在前端部分使用struts标签显示username的提示
	+ 继承ActionSupport里面的Validate()方法，也可以实现跳转
 ### 根据位置不同来划分处理结果类型：
 + 局部结果
 + 全局结果：所有的包下面的请求都可以公用其中返回功能

