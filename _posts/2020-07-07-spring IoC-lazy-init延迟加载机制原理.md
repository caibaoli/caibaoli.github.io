---
title: 2020-07-07-spring IoC-lazy-init延迟加载机制原理
author: baocai
categories: [spring]
tags: spring
---


# spring IoC 应用


### 第4节 lazy-init 延迟加载机制原理

 **2.1 lazy-init 延迟加载机制**

> 普通 Bean 的初始化是在容器启动初始化阶段执行的，
> lazy-init=true修饰的 bean 则是在从容器里第一次进行context.getBean() 时进行触发。

 - 对于被修饰为lazy-init的bean Spring 容器初始化阶段不会进行 init 并且依赖注入，当第一次 进行getBean时候才进行初始化并依赖注入

 - 对于非懒加载的bean，getBean的时候会从缓存里头获取，因为容器初始化阶段 Bean 已经 初始化完成并缓存了起来

