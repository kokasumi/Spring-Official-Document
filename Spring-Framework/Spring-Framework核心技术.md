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

- 使用XML格式配置。在XML顶级`<beans/>`元素中定义`<bean/>`元素。
- 基于注解配置：Spring 2.5引入了基于注解的配置元数据格式支持
- 基于Java代码配置：从Spring 3.0开始，Spring JavaConfig项目的很多功能都成为Spring Framework的一部分，可以使用Java代码代替XML定义外部bean。在`@Configuration`类中对应方法使用`@Bean`声明。

这些bean定义对应于构成应用程序的实际对象，通常会定义服务层对象，数据访问层对象(DAO)，如Struts `Action`实例之类的表现层对象，如Hibernate `SessionFactories`，JMS `Queues`之类的基础结构对象。通常不会在容器中配置细粒度的域对象，因为创建和加载域对象是DAOs和业务逻辑层的职责。但是也可以将Spring与AspectJ结合使用来配置在IOC容器控制范围外创建的对象。

```xml
<!--XML格式配置Bean-->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  	xsi:schemaLocation="http://www.springframework.org/schema/beans
    	http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="..." class="...">   
        <!-- collaborators and configuration for this bean go here -->
    </bean>
    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>
    <!-- more bean definitions go here -->
</beans>
```

#### 实例化容器

传递给`ApplicationContext`构造器的位置路径是资源字符串，使得容器能够从外部资源(如本地文件系统/Java `CLASSPATH`等)中加载配置元数据，如下代码所示:

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```

```xml
<!--服务层对象配置文件-->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  	xsi:schemaLocation="http://www.springframework.org/schema/beans
    	http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- services -->
    <bean id="petStore" 		class="org.springframework.samples.jpetstore.services.PetStoreServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <property name="itemDao" ref="itemDao"/>
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>
    <!-- more bean definitions for services go here -->
</beans>
```

```xml
<!--数据访问对象配置文件-->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="accountDao"
        class="org.springframework.samples.jpetstore.dao.jpa.JpaAccountDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>
    <bean id="itemDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaItemDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>
    <!-- more bean definitions for data access objects go here -->
</beans>
```

在上面的示例中，服务层由1个`PetStoreServiceImpl`类和2个类型为`JpaAccountDao`和`JpaItemDao`(基于JPA对象关系映射标准)的数据访问对象构成。属性`name`对应JavaBean的属性名，`ref`属性对应另一个Bean定义的名称，`id`和`ref`表达了协作对象之间的依赖关系。

##### 编写基于XML的配置元数据

跨越XML进行Bean定义是一种比较有用的方式，通常来说，每个单独的XML配置文件都代表了架构中的逻辑层或者模块。可以使用应用Context构造器从所有的XML文件中加载bean定义，该构造器可以传入多个`Resource`，或者使用1个或多个`<import/>`元素从其他xml文件加载bean定义。

```xml
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```

- 以上xml文件引入了3个xml文件(`service.xml`,`messageSource.xml`,`themeSource.xml`)定义的bean。
- 引入文件的位置都是相对于执行引入xml文件路径。
- 可以使用`../`来表示父文件目录的资源路径，但是不推荐，这样使得当前应用程序与外部文件创建依赖关系。特别不建议对`classpath:`URL使用。
- 可以使用资源的绝对路径来代替相对路径，但是需要注意使用`${...}`等占位符保持应用程序与资源绝对路径解耦。

##### Groovy Bean Definition DSL

外化配置元数据也可以使用Spring Grails之类的框架中Groovy Bean Definition DSL来表示，通常该类配置文件以`.groovy`结尾并采用如下结构表示：

```groovy
beans {
    dataSource(BasicDataSource) {
        driverClassName = "org.hsqldb.jdbcDriver"
        url = "jdbc:hsqldb:mem:grailsDB"
        username = "sa"
        password = ""
        settings = [mynew:"setting"]
    }
    sessionFactory(SessionFactory) {
        dataSource = dataSource
    }
    myService(MyService) {
        nestedBean = { AnotherBean bean ->
            dataSource = dataSource
        }
    }
}
```

这种配置样式在很大程度上类似于XML bean定义，甚至支持Spring的XML配置命名空间，还允许通过`importBeans`指令引入XML bean定义文件。

#### 使用容器

`ApplicationContext`是一种能够维护不同bean的注册表及其依赖关系的高级工厂接口，通过使用`T getBean(String name, Class<T> requiredType)`方法可以拿到bean实例。使用`ApplicationContext`能够读取并访问bean，如下所示：

```java
// create and configure beans
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// retrieve configured instance
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// use configured instance
List<String> userList = service.getUsernameList();
```

针对Groovy配置，与XML比较类似，其有另外一个了解Groovy(也明白XML bean定义)的`ApplicationContext`实现类。

```java
ApplicationContext context = new GenericGroovyApplicationContext("services.groovy", "daos.groovy");
```

最灵活的变体是`GenericApplicationContext`，能够与Reader代理相结合。比如，针对XML文件与`XmlBeanDefinitionReader`结合；或者针对Groovy文件使用`GroovyBeanDefinitionReader`。可以

```java
GenericApplicationContext context = new GenericApplicationContext();

//xml
new XmlBeanDefinitionReader(context).loadBeanDefinitions("services.xml", "daos.xml");
context.refresh();

//groovy
new GroovyBeanDefinitionReader(context).loadBeanDefinitions("services.groovy", "daos.groovy");
context.refresh();
```

可以在同一个`ApplicationContext`中混合匹配Reader代理，从不同的配置源中读取bean定义。使用`ApplicationContext`的`getBean()`之类的方法可以检索bean实例，但是在实际应用程序中，应该保持不调用这些方法和不依赖于Spring API的原则。如Spring和Web框架的集成为各种Web框架组件(如控制器和JSF托管bean)提供依赖注入，允许通过元数据声明对特定bean的依赖性。

### Bean简介

Spring IOC容器管理一个或者多个bean，这些bean使用提供给容器的配置元数据创建。在容器内部，这些bean定义表示为`BeanDefinition`对象，其中包括以下元数据：

- 完全限定的包类名：通常是被定义bean的实际实现类。
- Bean行为配置元素，描述了bean在容器中的行为方式(如范围，生命周期回调等)
- bean正常工作需要使用的其他bean引用，这些引用也称作协作者或者依赖项
- 新创建对象中的其他配置，如池的大小限制或者管理连接池中的使用bean连接数

这些元数据转换为构成bean定义的一组属性，其中的属性包括：

| Property                 | Explained in...                                              |
| ------------------------ | ------------------------------------------------------------ |
| Class                    | [Instantiating Beans](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-class) |
| Name                     | [Naming Beans](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-beanname) |
| Scope                    | [Bean Scopes](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-scopes) |
| Constructor arguments    | [Dependency Injection](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-collaborators) |
| Properties               | [Dependency Injection](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-collaborators) |
| Autowiring mode          | [Autowiring Collaborators](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-autowire) |
| Lazy initialization mode | [Lazy-initialized Beans](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-lazy-init) |
| Initialization method    | [Initialization Callbacks](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-lifecycle-initializingbean) |
| Destruction method       | [Destruction Callbacks](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-lifecycle-disposablebean) |

除了包含如何创建特殊bean的定义信息之外，`ApplicationContext`也允许注册容器外部创建的对象。这是通过`getBeanFactory()`方法访问ApplicationContext的BeanFactory实现的，该方法返回值为`DefaultListableBeanFactory`，支持通过`registerSingleton(..)`和`registerBeanDefinition(..)`方法来注册。

> 注：Bean元数据和手动提供的单例实例需要尽早注册，以便容器在自动装配及其他自查步骤期间能够合理的了解分配。虽然在某种程度上支持覆盖现有元数据和单例实例，在运行时注册新bean不是官方支持，可能会导致并发访问异常，bean容器中的状态不一致等。

#### Bean命名

每个bean都有一个或者多个标识符，这些 标识符在托管bean的容器中必须是唯一的。bean通常只有一个标识符，但如果需要多个标识符，其他的可以被视作别名。

在基于XML的配置元数据中，可以使用`id`属性，`name`属性或者两者来指定bean标识符。`id`属性允许指定一个id，`name`属性通常由字母/数字/特殊字符组成。如果想要给bean引入其他别名，可以使用`逗号(,)`，`分号(;)`或者空格分隔的字符串定义`name`属性。在Spring 3.1之前版本，`id`属性被定义成`xsd:ID`类型，约束了一些字符定义。从3.1版本开始，`id`属性就被定义成`xsd:string`类型，不再约束字符定义，bean ID唯一性仍由容器强制执行，但不再由XML解析器强制执行。

不一定需要给bean显示提供`id`和`name`属性，如果未明确提供`name`或者`id`，容器会给该bean生成唯一的标识。但如果想要通过名称引用该bean，通过使用`ref`元素或者Service Locator方式查找，必须提供名称标识。不提供名称的目的与使用[inner beans](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-inner-beans)和[autowiring collaborators](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-autowire)有关。

> Bean命名约定：使用标准java约定作为实例字段名称。也就是说，bean命名使用驼峰格式以小写字母开头，Bean命名始终需要遵守易于阅读理解的原则。

在定义bean时，可通过使用`id`属性和任意多`name`属性结合为bean提供过个名称