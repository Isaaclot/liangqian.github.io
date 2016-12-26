---
title: 在Github创建静态网站预览的方法
date: 2016-03-16 11:51:09
tags: 小技巧
---
+ 前段时间参加了百度前端学院的前端学习，想着不用把别人前端页面代码下载下来就能预览的话，就方便多了，既能看代码，又能看到效果，还可以用chrome调试
___
## 正文
+ 方法一：通过第三方开源项目预览
  + 访问`http://htmlpreview.github.io/`[enter link description here](http://htmlpreview.github.io/) 填写你要预览的页面地址，Enter-->Get
  + 得到这样格式的访问连接：`http://htmlpreview.github.io/?https://github.com/liangqian/ifeFPGetting/blob/master/Part1/task_01_01_01.html` 也就是 ：`http://htmlpreview.github.io/?`+目标预览页面地址
+ 方法二：创建gh-pages分支进行预览
  + 创建gh-pages分支进行预览，创建分支之后，访问`http://你的Github名.github.io/`+页面地址
  例如：liangqian.github.io/preview/test.html
  + 这是官方的推荐方法 [enter link description here](https://help.github.com/articles/creating-project-pages-manually/)
