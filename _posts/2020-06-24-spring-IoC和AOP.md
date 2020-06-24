---
title: 2020-06-24-spring-IoC和AOP
author: baocai
categories: [spring]
tags: spring
---


# IoC和AOP

### 第1节 解决new关键字问题
1、使用反射实例化对象
2、使用工厂通过反射技术生产对象

### 第2节 service层没有事务控制，出现异常可能导致数据错乱问题
1、数据库事务归根结底是Connection的事务，connection.commit(),connection.rollback()

### 第3节 JDK动态代理扩展

1、动态代理
静态代理：实实在在已存在一个代理
动态代理：不需为每个业务创建一个代理类，jdk动态代理/cglib动态代理，jdk动态代理要求委托对象必须实现接口，因为代理使用时必须传入接口。