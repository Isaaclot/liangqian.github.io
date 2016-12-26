---
title: Angular JS 基础笔记
date: 2016-07-15 00:48:37
tags: Angular JS
---
## AngularJS 开发
+ 使得开发现代的单一页面应用程序（SPAs：Single Page Applications）变得更加容易。
+ Angular JS 把应用程序数据绑定到HTML元素
+ Angular JS 可以克隆和重复HTML元素
+ Angular JS 可以隐藏/显示HTML元素
+ Angular JS 可以在元素背后添加代码
+ Angular JS 支持输入验证

## $scope 作用域
+ $scope 对象当作一个参数传递
+ 带有属性和方法，这些属性和方法可以在视图和控制器中使用
+ 根作用域`$rootScope`: 可作用于整个应用中。是各个 controller 中 scope 的桥梁。用 rootscope 定义的值，可以在各个 controller 中使用。

## Angular JS 服务
+ $location : 返回当前页面的 URL 地址 ($location.absUrl();)
+ $http :(例子)
```
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
    $http.get("welcome.htm").then(function (response) {
        $scope.myWelcome = response.data;
    });
});
```
+  $timeout 访问在规定的毫秒数后执行指定函数
```
$timeout(function () {
      $scope.myHeader = "How are you today?";
  }, 2000);
```
+ $interval 访问在指定的周期(以毫秒计)来调用函数或计算表达式
```
 $interval(function () {
      $scope.theTime = new Date().toLocaleTimeString();
  }, 1000);
```
+ 创建自定义服务
```
app.service('hexafy', function() {
    this.myFunc = function (x) {
        return x.toString(16);
    }
});
```
+ 使用自定义服务
```
app.controller('myCtrl', function($scope, hexafy) {
    $scope.hex = hexafy.myFunc(255);
}); //不加`$`区分自带和自定义服务
```



## Angular JS 指令
+ ng-app 指令初始化一个AngularJS应用程序
+ ng-init 初始化一个应用程序数据
+ ng-model 把元素值(比如输入域的值)绑定到应用程序中
+ ng-repeat 指令会重复一个HTML元素
```
<div ng-app="" ng-init="names=['Jani','Hege','dasd'];in=100">
  <p>使用 ng-repeat 来循环数组</p>
	<input type=number ng-model="in" >
  <ul>
    <li ng-repeat="x in names">
      {{ x }}
    </li>
	  <li>{{in}}</li>
  </ul>
</div>
```
+ ng-disabled : 直接绑定应用程序数据到HTML的disabled属性 ; 当 标记记作 `true` 被标记的元素失效
```
<button ng-disabled="mySwitch">点我!</button>
<input type="checkbox" ng-model="mySwitch">
```
+ ng-show : 被标记元素标记为true时，改元素可见
+ ng-hide :  true ---> 不可见

## Angular JS 事件
+ ng-click : 定义ng的点击触发事件
___
## 未完待续......

