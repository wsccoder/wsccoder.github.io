---
layout:     post
title:      Hello 2018
subtitle:    "\"First Blog In GitHub\""
date:       2018-08-13
author:     BY
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - 生活
    - 学习
    - GitHub
    - Coding
---



> “🙉🙉🙉 "

## 前言 

Github 的第一篇博客

同时学习使用[Typora] mackdown编写软件

（www.baidu.com）



 ### 正文



---

 #### 正文

**加粗**

*斜体*



下面是正文

---



`

**下面是代码块**



```java
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
            // Destroy already created singletons to avoid dangling resources.
            destroyBeans();

            // Reset 'active' flag.
            cancelRefresh(ex);

            // Propagate exception to caller.
            throw ex;
        }
    }
}`


```




```objc
@property (nonatomic, strong) NSString *target;
dispatch_queue_t queue = dispatch_queue_create("parallel", DISPATCH_QUEUE_CONCURRENT);
for (int i = 0; i < 1000000 ; i++) {
    dispatch_async(queue, ^{
    	// ‘ksddkjalkjd’删除了
        self.target = [NSString stringWithFormat:@"%d",i];
    });
}
```


