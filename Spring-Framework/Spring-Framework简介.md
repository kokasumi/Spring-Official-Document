# Spring Framework简介

> 原文：[Spring Framework Overview](https://docs.spring.io/spring/docs/5.1.4.RELEASE/spring-framework-reference/overview.html#overview)
>
> 整理者：[Mnilg](https://github.com/mnilg)

Spring能够很轻松的创建Java企业应用程序(Java Enterprise Application)，其提供了企业环境中Java语言需要的一切，也支持Groovy和Kotlin作为JVM中的替代语言，能够根据应用需求灵活创建不同的体系结构。

## 1.什么是"Spring"

通常来讲，"Spring"并不是指的一个事物，在不同的情景下代表的不同的东西。在刚开始时，"Spring"就是指的是[Spring Framework](https://github.com/spring-projects/spring-framework)；随着时间发展，基于Spring Framework又开发出了一系列的Spring项目，这时候通常指的是[Spring系列项目](https://spring.io/projects)。

Spring Framework分为多个模块，开发者可以根据需求选择需要的模块。最核心的就是spring-core中的内容，包括配置模型(Configuration Model)和依赖注入机制(Dependency Injection Mechanism)。除此之外，Spring Framework为不同的应用架构都提供了基础支持，包括消息传递/事务数据以及持久化/Web等。它还包括基于Servlet的Spring MVC Web框架和Spring WebFlu响应式Web框架。

