---
title: 2020-07-08-Spring中AOP实现
author: baocai
categories: [spring]
tags: spring
---


# Spring AOP 应用


### 第5节  Spring中AOP实现

**五种通知类型**

 1. 前置通知
 - aop:before
 - @Before
 2. 正常执行时通知
 - aop:after-returning
 - @AfterReturning
 3. 异常通知
   - aop:after-throwing
   - @AfterThrowing
 4. 最终通知
     - aop:after
     - @After
 5. 环绕通知：控制增强代码本身
     - aop:around
     - @Around
环绕通知，它是有别于前面四种通知类型外的特殊通知。
前面四种通知(前置，后置，异常和最终) 它们都是指定何时增强的通知类型。而环绕通知，它是Spring框架为我们提供的一种可以通过编码的 方式，控制增强代码何时执行的通知类型。它里面借助的ProceedingJoinPoint接口及其实现类， 实现手动触发切入点方法的调用。



