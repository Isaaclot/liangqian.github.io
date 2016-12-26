---
title: 关于Maven 打包时 缺失properties和xml后缀的文件
date: 2016-06-05 11:00:33
tags: Maven
---
+ 背景
  + Intellij IDEA 14.1.5
  + Maven 项目
+ 问题
### 打包Maven项目的时候，报错
```
log4j:WARN No appenders could be found for logger (logmain).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
```
### 但是我的目录下有`log4j.properties`配置文件
### 展开才发现：输出的类没有'log4j.properties'配置文件
## 解决方案
+ 在pom中添加resources配置
```
<build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                </includes>
            </resource>
        </resources>
    </build>
```
## 说明
+ Maven的生命周期：

|生命周期阶段|目标|
|---|---|
|process-resources|resources:resources|
|compile|compile:compile|
|process-test-resources|resources:testResource|
|test-compile|compile:testCompile|
|test|surefire:test|
|package|war:war|
|install|install:install|
|deploy|deploy:deploy|
+ 经测试

  1. 当pom中不增加resources配置时，
执行process-resources，class文件夹下只包含src/main/resources下的文件
执行compile，class文件夹下包含src/main/resources下的文件与src/main/java下的*.class文件，丢失src/main/java下的*.properties文件
  2. 当pom中增加resources配置时
执行process-resources，class文件夹下只包含src/main/resources下的文件与src/main/java下的*.properties文件
执行compile，class文件夹下包含src/main/resources下的文件与src/main/java下的*.class文件与*.properties文件

### 同样，对于maven项目加载不出来`xml`文件的情况，解决方法类似


```
<build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </resource>
        </resources>
</build>```
