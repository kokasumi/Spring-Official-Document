# Spring Framework简介

> 原文：[Spring Framework Overview](https://docs.spring.io/spring/docs/5.1.4.RELEASE/spring-framework-reference/overview.html#overview)
>
> 整理者：[Mnilg](https://github.com/mnilg)

Spring能够很轻松的创建Java企业应用程序(Java Enterprise Application)，其提供了企业环境中Java语言需要的一切，也支持Groovy和Kotlin作为JVM中的替代语言，能够根据应用需求灵活创建不同的体系结构。

## 1.什么是"Spring"

通常来讲，"Spring"并不是指的一个事物，在不同的情景下代表的不同的东西。在刚开始时，"Spring"就是指的是[Spring Framework](https://github.com/spring-projects/spring-framework)；随着时间发展，基于Spring Framework又开发出了一系列的Spring项目，这时候通常指的是[Spring系列项目](https://spring.io/projects)。

Spring Framework分为多个模块，开发者可以根据需求选择需要的模块。最核心的就是spring-core中的内容，包括配置模型(Configuration Model)和依赖注入机制(Dependency Injection Mechanism)。除此之外，Spring Framework为不同的应用架构都提供了基础支持，包括消息传递/事务数据以及持久化/Web等。它还包括基于Servlet的Spring MVC Web框架和Spring WebFlu响应式Web框架。

## 2.Spring和Spring Framework的历史

Spring成立于2003年，是对早期[J2EE](https://en.wikipedia.org/wiki/Java_Platform,_Enterprise_Edition)规范复杂性的补充。Spring编程模型并不包含所有的Java EE平台规范，它集成了几种精挑细选出的EE个性化规范：

- Servlet API ([JSR 340](https://jcp.org/en/jsr/detail?id=340))
- WebSocket API ([JSR 356](https://www.jcp.org/en/jsr/detail?id=356))
- Concurrency Utilities ([JSR 236](https://www.jcp.org/en/jsr/detail?id=236))
- JSON Binding API ([JSR 367](https://jcp.org/en/jsr/detail?id=367))
- Bean Validation ([JSR 303](https://jcp.org/en/jsr/detail?id=303))
- JPA ([JSR 338](https://jcp.org/en/jsr/detail?id=338))
- JMS ([JSR 914](https://jcp.org/en/jsr/detail?id=914))
- 必要时用于事务协调的JTA/JCA设置

Spring Framework还支持依赖注入(Dependency Injection,[JSR 330](https://www.jcp.org/en/jsr/detail?id=330))和Common Annotations([JSR 250](https://jcp.org/en/jsr/detail?id=250)规范，可供开发者选择替代Spring Framework提供的Spring特定机制。

从Spring Framework 5.0开始，Spring至少需要Java EE 7(如Servlet 3.1+,JPA 2.1+)以上支持，同时在运行时提供与Java EE 8(如Servlet 4.0，Json Binding API)级别的新API开箱即用。这使得Spring完全兼容如Tomcat 8和9，WebSphere 9，JBoss EAP 7等。

随着时间发展，Java EE在应用开发中的角色也随之改变。在Java EE和Spring早期，应用程序需要在部署到应用服务器时创建。如今在Spring boot的帮助下，应用程序以devopts和cloud-friendly方式创建，几乎没有任何改动嵌入到Servlet容器中。在Spring Framework 5之后，WebFlux应用甚至直接不使用Servlet API并在不是Servlet容器(如Netty)的服务器上运行。

随着Spring持续创新发展，在Spring Framework基础上研发出了很多其他项目，如Spring Boot，Spring Security，Spring Data，Spring Batch等，每个项目都有自己的源码仓库，issue跟踪和发布节奏。具体的可以查看[spring.io/projects](https://spring.io/projects) 。

## 3.设计理念

- 在每个层次提供多个选择，使得Spring可以尽可能晚的延缓设计决策。如，可以通过通过配置无更改代码的切换持久性存储提供者。
- 适应不同的观点。Spring具有灵活的包容性，并不会固定认为应该怎么做，这样能够以不同的视觉观点适应广泛的应用需求。
- 保持强大的向后兼容性。Spring的演变经过精心管理强制在版本之间尽可能少的做出突破性的变化(breaking changes)。Spring支持的JDK版本以及第三方库都是经过精挑细选的，便于维护依赖于Spring的应用和库。
- 关注API设计。Spring团队花了很大心思和时间编写直观的API，并且能够在多个版本很多年之后都能够保持使用。
- 高标准的代码质量。Spring Framework特别重视有意义，最新，准确的javadoc，是极少数能够声称干净的代码结构，包之间没有任何循环依赖的项目之一。

## 4.入门

- 使用[start.spring.io](https://start.spring.io/)生成一个基本的Spring 项目
- 按照[入门指南](https://spring.io/guides)学习
- 