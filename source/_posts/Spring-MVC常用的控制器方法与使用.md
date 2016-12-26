---
title: Spring MVC常用的控制器方法与使用
date: 2016-07-19 00:27:20
tags: Spring
---
## Spring mvc 控制器与方法
#### @Controller定义一个Controller控制器
+ 在使用这个标记主注解之前需要配置控制器(两种方法)	
	+ 在Spring MVC配置文件中添加`<bean>`对象：`<bean class="xyz.royleo.vice"/>`
	+ 在spring配置文件中@Controller控制器的位置：
```
< context:component-scan base-package = "com.host.app.web.controller" >  
   < context:exclude-filter type = "annotation"  
       expression = "org.springframework.stereotype.Service" />  
</ context:component-scan >  
```
#### @RequestMapping来映射Request请求与控制器
+ 通过URL模板实现 : 在URL模板中含有变量值,再在`@requestMapping`的方法里面，通过`@PathVariable`获取通过RUL传过来的值
```
@Controller  
@RequestMapping ( "/test/{variable1}" )  
public class MyController {  
    @RequestMapping ( "/showView/{variable2}" )  
    public ModelAndView showView( @PathVariable String variable1, @PathVariable ( "variable2" ) int variable2) {  
		//do something 
       return modelAndView;  
    }  
}   
```
+ 使用`@RequestParam`绑定HttpServletRequest请求参数到控制器方法参数
	+ 其中，有requested=false是可选的参数，默认是必须的
```
@RequestMapping ( "requestParam" )  
ublic String testRequestParam( @RequestParam(required=false) String name, @RequestParam ( "age" ) int age) {  
   return "requestParam" ;  
}  
```

#### HttpServletRequest & HttpServletResponse & HttpSession...
+ 跟普通的Servlet一样，spring mvc中也可以用servlet里面的方法
```
@RequestMapping("/somePath")
   public String requestResponseExample(HttpServletRequest request, HttpServletResponse reponses, HttpSession session, Model model) {
	 // Todo something here
       return "someView";
   }
```
#### 控制器的重定向
+ 使用前缀：`redirect`该方法返回字符串，可以重定向到另一页面
```
@RequestMapping(value = "/redirect", method = RequestMethod.GET)
    public String authorInfo(Model model) {
	 // Do somethong here
	  return "redirect:/hello";
    }
```
#### 使用`@CookieValue`绑定cookie数值到Controller方法参数
```
    @RequestMapping ( "cookieValue" )
    public String testCookieValue( @CookieValue ( "hello" ) String cookieValue, @CookieValue String hello) {
	    //do something
       return "cookieValue" ;
    } 
```
#### 使用`@RequestHeader`注解绑定HttpServletRequest头信息到Controller方法参数
```
@RequestMapping ( "testRequestHeader" )
public String testRequestHeader( @RequestHeader ( "Host" ) String hostAddr, @RequestHeader String Host, @RequestHeader String host ) {
   //use Host... do something
    return "requestHeader" ;
} 
```
#### `@RequestMapping`的一些高级应用
+ RequestMapping除了指定的路径value值之外，还有其他的一些属性，如：params，method，headers等,便于缩小映射范围
+ `params`属性：
	+ 例子说明：代码中定义了映射的路径，params的三个参数，当映射路径`/testParams.do`并且满足params的规则(param1等于value1，param2必须存在，值无所谓，param3必须不存在)，才能正确映射到testParams()方法. 即：`/testParams.do?param2=value&param2=value2`才能访问指定方法
```
@RequestMapping (value= "testParams" , params={ "param1=value1" , "param2" , "!param3" })  
public String testParams() {  
   System. out .println( "test Params..........." );  
   return "testParams" ;  
}  
```
+ method属性:主要用于限制能访问的方法类型(GET,POST,DELETE...)
```
@RequestMapping (value= "testMethod" , method={RequestMethod. GET , RequestMethod. DELETE })  
public String testMethod() {  
   return "method" ;  
}   
```
+ `headers`属性
	+ headers 属性的用法和功能与params 属性相似。在上面的代码中当请求/testHeaders.do 的时候只有当请求头包含Accept 信息，且请求的host 为localhost 的时候才能正确的访问到testHeaders 方法。
```
@RequestMapping (value= "testHeaders" , headers={ "host=localhost" , "Accept" })  
public String testHeaders() {  
   return "headers" ;  
}   
```
#### 以 @RequestMapping 标记的处理器方法支持的方法参数和返回类型
+ 支持的方法参数类型
	+ HttpServlet对象：HttpServletRequest、HttpServletResponse、HttpSession
	+ Spring自己的WebRequest对象
	+ InputStream、OutputStream、read、write
	+ 使用 @pathVariable @RequestParam @CookieValue @RequestHeader 标记的参数
	+ 使用 @ModelAttribute 标记的参数
	+ java.util.Map 、Spring 封装的Model 和ModelMap：用来展现视图
	+ 实体类
	+ Spring封装的MutipartFile,用来上传文件
	+ Spring 封装的Errors 和BindingResult 对象
+ 支持的返回类型
	+ 一个包含模型和视图的ModelAndView 对象
	+ 一个模型对象，这主要包括Spring 封装好的Model 和ModelMap ，以及java.util.Map ，当没有视图返回的时候视图名称将由RequestToViewNameTranslator 来决定
	+ 一个View 对象。这个时候如果在渲染视图的过程中模型的话就可以给处理器方法定义一个模型参数，然后在方法体里面往模型中添加值
	+ 一个String字符串、返回JSON对象(主要是Restful风格，前后端通信)
	+ 返回值是void：主要方法体接受传值，执行相关逻辑，返回值可以忽略
	+ 如果处理器方法被注解@ResponseBody 标记的话，那么处理器方法的任何返回类型都会通过HttpMessageConverters 转换之后写到HttpServletResponse 中，而不会像上面的那些情况一样当做视图或者模型来处理
	+ 除以上几种情况之外的其他任何返回类型都会被当做模型中的一个属性来处理，而返回的视图还是由RequestToViewNameTranslator 来决定，添加到模型中的属性名称可以在该方法上用@ModelAttribute(“attributeName”) 来定义，否则将使用返回类型的类名称的首字母小写形式来表示。使用@ModelAttribute 标记的方法会在@RequestMapping 标记的方法执行之前执行
#### 使用 @ModelAttribute 和 @SessionAttributes 传递和保存数据
+ @ModelAttribute标记一般放在需要标记的上面，并指定相关值，当@RequestMapping调用属性参数获取的时候执行
+ @SessionAttribute 标记一般放在@RequestMapping注解的下面，类名的上面。被@SessionAttribute标记的参数的数值需要被添加到Session中之后，在读取出来之后才有内容；当然，添加的标记的参数类型可以是字符串，数组，实体对象等

