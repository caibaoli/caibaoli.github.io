---
title: 2020-06-28-spring IoC-源码深度剖析
author: baocai
categories: [spring]
tags: spring
---


# spring IoC 应用


### 第1节 Spring IoC容器初始化主体流程

 **1.1 容器体系**
 
 BeanFactory 容器继承体系
![walking]({{ site.baseurl }}/assets/images/ioc_aop/BeanFactory容器继承体系.png)

> Bean 的创建是在容器初始化时，在设置延迟加载的情况下，Bean 的创建是在getBean()时。

 **1.2 生命周期关键点**

 Ioc容器创建管理Bean对象的，Spring Bean是有生命周期的
 - 构造器执行、初始化方法执行、Bean后置处理器的before/after方法: AbstractApplicationContext#refresh#finishBeanFactoryInitialization.
 - Bean工厂后置处理器初始化、方法执行: AbstractApplicationContext#refresh#invokeBeanFactoryPostProcessors.
 - Bean后置处理器初始化：AbstractApplicationContext#refresh#registerBeanPostProcessors.

**1.3 Spring IoC容器初始化主流程**
 

``` aspectj
@Override
	public void refresh() throws BeansException, IllegalStateException {
		synchronized (this.startupShutdownMonitor) {
			// Prepare this context for refreshing.
			prepareRefresh();

			// Tell the subclass to refresh the internal bean factory.
			ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

			// Prepare the bean factory for use in this context.
			prepareBeanFactory(beanFactory);

			try {
				// Allows post-processing of the bean factory in context subclasses.
				postProcessBeanFactory(beanFactory);

				// Invoke factory processors registered as beans in the context.
				invokeBeanFactoryPostProcessors(beanFactory);

				// Register bean processors that intercept bean creation.
				registerBeanPostProcessors(beanFactory);

				// Initialize message source for this context.
				initMessageSource();

				// Initialize event multicaster for this context.
				initApplicationEventMulticaster();

				// Initialize other special beans in specific context subclasses.
				onRefresh();

				// Check for listener beans and register them.
				registerListeners();

				// Instantiate all remaining (non-lazy-init) singletons.
				finishBeanFactoryInitialization(beanFactory);

				// Last step: publish corresponding event.
				finishRefresh();
			}

			catch (BeansException ex) {
				if (logger.isWarnEnabled()) {
					logger.warn("Exception encountered during context initialization - " +
							"cancelling refresh attempt: " + ex);
				}

				// Destroy already created singletons to avoid dangling resources.
				destroyBeans();

				// Reset 'active' flag.
				cancelRefresh(ex);

				// Propagate exception to caller.
				throw ex;
			}

			finally {
				// Reset common introspection caches in Spring's core, since we
				// might not ever need metadata for singleton beans anymore...
				resetCommonCaches();
			}
		}
	}
```

