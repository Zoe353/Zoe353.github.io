---
layout: post
title: "05/15/2020-Spring-Framework"
markdown: kramdown
date: 2020-05-15
---
Spring框架中的核心组件有三个：Core、Context和Beans。
Beans非常重要。Spring是面向Bean的编程（BOP, Bean Oriented Programming）。 Bean在Spring中作用就像Object对OOP的意义一样。SpringSpring解决了一个非常关键的问题，用配置文件来管理对象之间的依赖关系（依赖注入机制）。而这个注入关系在一个叫Ioc容器中管理，那Ioc容器就是被
Bean包裹的对象。Spring正式通过把对象包装在Bean中从而达到对这些对象的管理以及一些额外操作的目的。  
Bean包装的是Object，而Object必然有数据，如何给这些数据提供生存环境就是Context要解决的问题，对Context来说他就是要发现每个Bean之间的关系，为他们建立这种关系
并且要维护好这种关系。所以Context就是一个Bean关系的集合，这个关系集合又叫做Ioc容器，一旦建立起Ioc容器，Spring就可以工作了。Core的用途是发现、建立和维护每个Bean之间的关系所需要的一些列的工具。  

Bean组件在Spring的org.springframework.beans包下。这个包下的所有类主要解决了三件事：Bean的定义、创建以及解析。Spring的使用者唯一需要关心的就是Bean的床见，其他两个由
Spring在内部完成了。  
Context 在 Spring 的 org.springframework.context 包下，他实际上就是给 Spring 提供一个运行时的环境，用以保存各个对象的状态。  
Core 组件作为 Spring 的核心组件，他其中包含了很多的关键类，其中一个重要组成部分就是定义了资源的访问方式。这种把所有资源都抽象成一个接口的方式很值得在以后的设计中拿来学习。
Spring中AOP特性：动态原理， AOP如何实现的（待补充）  
Spring中设计模式：工厂模式、单例模式、模板模式（待补充）  
策略模式（待补充）  

链接：https://www.ibm.com/developerworks/cn/java/j-lo-spring-principle/index.html  
