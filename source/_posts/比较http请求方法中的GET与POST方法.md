layout: w
title: 比较http请求方法中的GET与POST方法
date: 2016-07-19 18:41:35
tags: [Servlet,Web]
---
## GET方法和POST方法的异同
+ GET方法：向页面请求发送已编码的用户信息；页面与已编码信息之间用`?`隔开传递:`http://localhost/test?key1=value&key2=value2`
    + GET方法的特点是将需要传递信息暴露在URL上面，因此不宜传输敏信息
    + 同时，GET方法有大小限制：请求字符串最多只能有1024个字符
+ POST方法：通过比较可靠的方式向后台程序传递数据；但是在传递方式上与GET方法不同，POST方法是通过标准输的形式将一个单独的消息传递给后台程序
## 在后台程序中读取传递过来的数据
+ 解析方法：
    + String getParameter(String str); 获取表单参数值
    + String[] getParameterValues(String str); 获取在同一个参数里多个值,并以数组形式返
    + String[] getParameterNames();获取请求中所有参数的完整列表
