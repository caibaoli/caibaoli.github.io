---
title: 2020-07-08-spring IoC-Spring IoC循环依赖问题
author: baocai
categories: [spring]
tags: spring
---


# spring IoC 应用


### 第5节 Spring IoC循环依赖问题

 **Spring IoC循环依赖问题**
 
 - 什么是循环依赖：A依赖于B，B依赖于C，C又依赖于A。

> 这里不是函数的循环调用，是对象的相互依赖关系

 ==**Spring中循环依赖场景有：**==

 1. 构造器的循环依赖(构造器注入)
 2. Field 属性的循环依赖(set注入)

> 其中，构造器的循环依赖问题无法解决，只能拋出 BeanCurrentlyInCreationException 异常；
> 解决属性循环依赖时，spring采用的是提前暴露对象的方法；

 - **【注】** prototype 原型 bean循环依赖(无法解决)
   
 
 解决方案：

Spring通过setXxx或者@Autowired方法解决循环依赖其实是通过**提前暴露一个ObjectFactory对象**来完成的。
简单来说ClassA在调用构造器完成对象初始化之后，在调用ClassA的setClassB方法 之前就把ClassA实例化的对象通过ObjectFactory提前暴露到Spring容器中。
举例：

 1. Spring容器初始化ClassA通过构造器初始化对象后提前暴露到Spring容器。
 2. ClassA调用setClassB方法，Spring首先尝试从容器中获取ClassB，此时ClassB不存在Spring 容器中。
 3. Spring容器初始化ClassB，同时也会将ClassB提前暴露到Spring容器中 
 4. ClassB调用setClassA方法，Spring从容器中获取ClassA ，因为第一步中已经提前暴露了ClassA，因此可以获取到ClassA
 5. 实例 ClassA通过spring容器获取到ClassB，完成了对象初始化操作。
 6. 这样ClassA和ClassB都完成了对象初始化操作，解决了循环依赖问题。

 
 