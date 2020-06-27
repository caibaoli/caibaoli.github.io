---
title: 2020-06-27-spring IoC 应用-高级特性
author: baocai
categories: [spring]
tags: spring
---


# spring IoC 应用


### 第2节 Spring IoC高级特性

 **1.1 lazy-Init 延迟加载**
ApplicationContext 容器的默认行为是在启动服务器时将所有 singleton bean 提前进行实例化。提前 实例化意味着作为初始化过程的一部分，ApplicationContext 实例会创建并配置所有的singleton bean。
如果不想让一个singleton bean 在 ApplicationContext实现初始化时被提前实例化，那么可以将bean
设置为延迟实例化。

``` swift
<bean id="testBean" calss="cn.lagou.LazyBean" lazy-init="true" />
```
容器层次

```  swift
<beans default-lazy-init="true">
    
</beans>
```

**应用场景**
(1)开启延迟加载一定程度提高容器启动和运转性能
(2)对于不常使用的 Bean 设置延迟加载，这样偶尔使用的时候再加载，不必要从一开始该 Bean 就占 用资源

 **1.2 FactoryBean 和 BeanFactory**
 BeanFactory接口是容器的顶级接口，定义了容器的一些基础行为，负责生产和管理Bean的一个工厂，
具体使用它下面的子接口类型，比如ApplicationContext。初始化及生成的Bean都可以在BeanFactory中找到。

FactoryBean是工厂Bean，创建的Bean的另一种方式，『直观上代替构造器和set方法代码，xml中需要配置』。目标Bean的创建类实现FactoryBean<T> 接口，可以达到创建Bean的目的。
 

``` aspectj
// 可以让我们自定义Bean的创建过程(完成复杂Bean的定义) 
public interface FactoryBean<T> {
@Nullable
// 返回FactoryBean创建的Bean实例，如果isSingleton返回true，则该实例会放到Spring容器 的单例对象缓存池中Map
 T getObject() throws Exception;
@Nullable
// 返回FactoryBean创建的Bean类型 
Class<?> getObjectType();
// 返回作用域是否单例
default boolean isSingleton() {
    return true;
  }
}
```

注：获取FactoryBean产生的对象，直接id，获取FactoryBean产生的对象上的工厂，id前加&。


 **1.3 FactoryBean 和 BeanFactory**
 
 Spring提供了两种后处理bean的扩展接口，分别为 BeanPostProcessor 和 BeanFactoryPostProcessor，两者在使用上是有所区别的。
 

> BeanPostProcessor—> 针对Bean对象，默认是对整个Spring容器中所有的bean进行处理，也可以针对某个具体的Bean.
> BeanFactoryPostProcessor -> 针对BeanFactory，BeanFactory级别的处理，是针对整个Bean的工厂进行处理，典型应用:PropertyPlaceholderConfigurer.

**Bean生命周期**
xml -> BeanDefinition -> Bean

> BeanDefinition对象:我们在 XML 中定义的 bean标签，Spring 解析 bean 标签成为一个 JavaBean， 这个JavaBean 就是 BeanDefinition

Spring Bean生命周期
![walking]({{ site.baseurl }}/assets/images/ioc_aop/Spring Bean生命周期.png)

BeanFactoryPostProcessor作用位置，封装成BeanDefinition还未Bean实例化时
![walking]({{ site.baseurl }}/assets/images/ioc_aop/BeanFactoryPostProcessor作用位置.png)



