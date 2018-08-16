# Spring

目录：

- [Spring](#spring)
- [Spring框架组成](#spring框架组成)

## Spring

Spring是一个开源的Java轻量级框架，非侵入性。

优点：

- 可简化企业应用开发的复杂性。
- 能够创建出松耦合、易测试、易扩展、易维护的Java应用系统。
- 开源框架，开放性较高。
- 有效地组织中间层对象。
- 多种可选的事务处理方式。
- 多种可选的持久层策略。
- 多种可选的Web MVC框架策略。
- 高度可扩展的安全解决方案。
- 有效的消除单例、工厂等模式的使用。
- 将面向接口编程做到实处。
- 使单元测试变得简单。
- 使EJB的使用成为一个选择。
- 提供了一致的数据访问框架。
- 只选择你需要的。

## Spring框架组成

如下图：

![frameworkComposition](https://raw.githubusercontent.com/lcfu1/Note/master/JavaWeb/image/frameworkComposition.PNG)

### 核心容器

- 核心容器提供Spring框架的基本功能,为Spring提供了基 础服务支持，核心容器的主要组件就是BeanFactory。
- BeanFactory是所有基于Spring框架系统的核心，通过 BeanFactory，Spring使用工厂模式来实现IoC，将应用程序的配置和依赖关系与实际的应用程序分离开来。
- Spring之所以称为容器，就是由于BeanFactory的自动装 备和注入。

### ApplicationContext（上下文）

- 由一个配置文件，向Spring框架提供上下文信息。
- BeanFactory使spring成为容器，上下文模块使Spring成 为框架。
- 这个模块对BeanFactory进行了扩展，添加了对I18N，系统生命周期事件以及验证的支持。这个模块提供了许多企业级服务，例如：邮件服务，JNDI访问，EJB集成，远程调用以及定时服务，并且支持与模板框架的集成。

### Spring AOP

- AOP面向切面编程，该模块将AOP编程功能集成到了Spring框架中。这样，凡是Spring框架管理的任何对象都可以很容易地支持AOP。该模块为应用程序中基于Spring管理的对象提供了事务管理服务。
- 这个模块由于使用了AOP Alliance的API，所以，可以和其他AOP框架互通。
- AOP Alliance是一个开源项目，目的是促进AOP的使用，并且通过定义一套通用的接口和组件来确保不同的AOP之间达到互通性。

### Spring DAO

- 这个模块封装了数据库连接的创建、语句对象生成、结果集处理、连接关闭等操作，而且重构了所有数据库系统的异常信息，用户不再需要处理数据库异常了。
- 在这个模块中，利用了Spring的AOP模块完成了为系统中对象提供事务管理的服务。

### Spring ORM

- Spring没有实现自己的ORM方案，而是为当前主流的ORM框架预留了整合接口，包括hibernate、JDO等。所有这些都遵从Spring的通用事务和DAO异常层次结构。
- Spring的事物管理支持所有这些ORM框架以及JDBC。

### Spring Web

- Web上下文模块建立在应用程序上下文模块之上，为基于Web的应用程序提供了上下文。
- Spring框架支持与Jakarta Struts的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
- MVC框架是一个全功能的构建Web应用程序的MVC实现。
- 通过策略接口，MVC 框架变成为高度可配置的。MVC 容纳了大量视图技术，其中包括JSP、Velocity、Tiles、iText和POI等。