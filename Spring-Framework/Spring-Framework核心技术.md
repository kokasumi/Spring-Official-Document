# Spring Framework核心技术

> 原文：[Spring Core Technologies](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#spring-core)
>
> 整理者：[Mnilg](https://github.com/mnilg)

## IOC容器

Spring Framework中最重要的就是控制反转(Inversion of Control，IOC)容器，IOC是紧随着Spring的面向切片编程(Aspect-Oriented Programming，AOP)技术全面普及发展起来的。Spring Framework有自己的AOP框架，其在概念上容易理解并且覆盖了Java企业编程中80%的AOP要求点。

### Spring IOC容器和Bean简介

在软件工程领域，**IOC(Inversion Of Control)**是一种计算机编程流控制方式。使用这种方式，自定义编写的计算机程序部分接受来自通用框架的控制流。与传统的程序编程相比，该种设计的软件架构颠倒了控制流：在传统编程中，都是通过在类内部主动创建依赖对象，导致类与类之间高度耦合；但是使用控制反转，将创建和查找依赖的权限交给容器，再由容器注入组合对象，这样对象与对象之间松散耦合，方便测试并利于功能复用。

IOC最主要的实现方式是通过**依赖注入(Dependency Injection，DI)**，根据IOC原则，对象实例定义的依赖对象只能通过外部参数传入(构造器参数/工厂方法参数/在对象实例通过构造器或者工厂方法返回后通过set方法设置属性)，然后在容器创建Bean时注入这些依赖。

`org.springframework.beans`和`org.springframework.context`是Spring Framework IOC容器基础。[`BeanFactory`](https://docs.spring.io/spring-framework/docs/5.1.4.RELEASE/javadoc-api/org/springframework/beans/factory/BeanFactory.html)接口提供了一种能够管理任何类型对象的高级配置机制，提供配置框架和基本功能；[`ApplicationContext`](https://docs.spring.io/spring-framework/docs/5.1.4.RELEASE/javadoc-api/org/springframework/context/ApplicationContext.html)是`BeanFactory`的子类，在`BeanFactory`基础上提供了更多企业特性，包括：

- 更容易与Spring AOP功能集成
- 消息资源处理(用于国际化)
- 事件发布
- 特定的应用层Context，如Web应用程序中的WebApplicationContext

在Spring中，构成应用程序主干并由Spring IOC容器管理的对象称为Bean，Bean是一个由Spring IOC容器实例化，组装并管理的对象。Bean及其依赖关系反映在容器使用的配置元素中。

### 容器简介

`org.springframework.context.ApplicationContext`接口代表了Spring IOC容器，负责实例化，配置，组装bean。容器通过读取配置元数据获取相关实例化，配置和组装对象的指令。配置元数据以XML，Java注解以及Java代码表示，能够表达组成应用程序的对象以及对象之间的依赖关系。

Spring提供了几个`AppicationContext`接口实现，在独立应用中，通常会创建[`ClassPathXmlApplicationContext`](https://docs.spring.io/spring-framework/docs/5.1.4.RELEASE/javadoc-api/org/springframework/context/support/ClassPathXmlApplicationContext.html)或者[`FileSystemXmlApplicationContext`](https://docs.spring.io/spring-framework/docs/5.1.4.RELEASE/javadoc-api/org/springframework/context/support/FileSystemXmlApplicationContext.html)对象实例。XML是定义配置元数据的传统格式，但是也可以使用少量XML配置来启用其他元数据格式(Java代码或者Java注解)的支持，并使用Java代码或注解作为元数据格式来指导容器。

![The Spring IOC container](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/images/container-magic.png)

以上图片展示了Spring的工作原理，在`ApplicationContext`创建初始化之后，应用类和配置元数据结合产生一个完全可配置执行的系统或应用。

#### 配置元素(Configuration Metadata)

配置元数据告诉Spring容器在应用中如何实例化，配置以及组合对象。配置元数据有3种不同的配置方式：

- 使用XML格式配置
- 基于注解配置：Spring 2.5引入了基于注解的配置元数据格式支持
- 基于Java代码配置：从Spring 3.0开始，Spring JavaConfig项目的很多功能都成为Spring Framework的一部分，可以使用Java代码代替XML定义外部bean