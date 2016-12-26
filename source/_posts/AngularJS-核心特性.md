---
title: AngularJS 核心特性
date: 2016-07-16 16:26:00
tags: Angular JS
---
+ MVC : Model - View - Controller
	+ 切分代码职责
	+ 更好的复用代码
	+ 便于模块化管理
	+ 终极目标：实现模块化，复用
+ 模块化Module：一起都是从模块开始，其他都是在module的基础上调用
+ 指令系统 
```
angular.module("mymodule",[]).directive("hello",function(){
	return {
		restrict: "E",
		template: '<div>Hi everyone</div>'
		replace: true
	}
})
```
+ 双向数据绑定
	+ 单向数据绑定：先定义好模板跟数据结合，通过数据绑定机制，将模板跟数据生成一段html标签，再把生成的HTML标签加入到文档流中，但是当数据变化的时候，再去改变view页面的数据，复杂度高；并且view中的数据变换(输入)的时候，无法将改变传回来给model数据
![单向数据绑定](http://7xqvf0.com1.z0.glb.clouddn.com/%E5%8D%95%E5%90%91%E6%95%B0%E6%8D%AE%E7%BB%91%E5%AE%9A.png)
	+ 双向数据绑定：view中的数据变化的时候，数据模型发生变化，当model中的数据发生变化的时候，其中改变能直接反馈给view页面
![双向数据绑定](http://7xqvf0.com1.z0.glb.clouddn.com/%E5%8F%8C%E5%90%91.png)
