---
title: 无法连接远程Mysql数据库的解决办法 I
date: 2016-05-10 23:02:32
tags: 小笔记
---
## tips: "Can not connet to mysql  server on '192.168.2.3' (10061)"
+ 很多时候想要远程登陆Mysql服务器，进行操作，发现弹出上面的提示...
## 解决方案：
  + 可能没有启动Mysql服务：`/etc/init.d/mysql start` 启动服务
  + 查看配置文件 `/etc/mysql/my.cnf` 文件中绑定的ip地址是否为默认ip(127.0.0.1)
```
root@ubuntu: vim /etc/mysql/my.cnf
root@ubuntu:                      #修改my.cnf的文件里面的配置
修改 bind-adress = 127.0.0.1
为 bind-adress = 服务器IP
#这种情况最显著的现象是：远程登陆不上Mysql服务器，但是服务器本地能登陆进去mysql程序
```
  + 检查或者重置my.ini配置文件是否正确
  + Mysql服务器连接池已满，重新释放一下连接池，这时候你要重新评估你的连接数或者检测是否有恶意访问攻击什么的


---
  如果找partner，要物色好可靠的人选；不然，两人的价值观不一样，做事情的效率跟质量都不一样。
