# RedPacket
基于SSM框架，使用Redis实现抢红包的高并发应用。


以下为该项目实现过程中的读书笔记。


互联网系统和传统的管理系统的区别：

移动互联网的新要求：

----高并发

----高响应

----数据一致性

----技术复杂化：在互联网中流行许多新技术，比如常见的NoSQL（Redis、MongoDB），又如MQ、RPC框架、ZooKeeper、大数据，分布式等技术。

Spring的核心是IoC（控制反转），他是一个大容器，方便组装和管理各类系统内外部的资源，同时支持AOP（面向切面编程），这是对面向对象的补充，目前广泛用于日志和数据库事务控制减少了大量的重复代码，使得程序更为清晰。

NoSQL的成功在于，首先它是基于内存的，也就是数据存放在内存中，而不是像数据库那样把数据放在磁盘上，而内存的读取速度是磁盘速度的几十倍到上百倍，所以NoSQL工具的速度远比数据库读取速度要快得多，满足了高响应的要求。即使NoSQL将数据放到磁盘中，它也是一种半结构化的数据格式，读取到解析的复杂度远比数据库要简单，这是因为数据库存储是经过结构化、多范式等有复杂规则的数据，还原为内存结构的速度较慢。NoSQL在很大程度上满足了高并发、快速读写响应。



第 一 章 认识SSM框架和Redis

它的成功来源于理念，而不是技术本身，它的理念包括IoC（Inversion of Control）和AOP（Aspect Oriented Programming）。



第 九 章 Spring IoC的概念

        它的成功来源于理念，而不是技术本身，它的理念包括IoC（Inversion of Control）和AOP（Aspect Oriented Programming）。

实现IoC的方式有依赖查找、依赖注入（构造器注入、setter注入、接口注入（其实一直没有明白什么是接口注入JNDI））、依赖拖拽；实现AOP的拦截功能的方式：使用ProxyFactoryBean和对应的接口实现AOP、使用XML配置AOP、使用@AspectJ注解驱动切面、使用AspectJ注入切面。

       Spring从2004年第一个版本至今已经十多年了。

装配Bean的方式：在XML中显示配置、在Java的接口和类中实现配置、隐式Bean的发现机制和自动装配原则。



第十一章 

      Spring对AOP的支持，在Spring中有四种方式去实现AOP的拦截功能。使用ProxyFactoryBean和对应的接口实现AOP、使用XML配置AOP、使用@AspectJ注解驱动切面、使用AspectJ注入切面。



第十二章

       Spring最重要的功能毫无疑问就是操作数据。数据库的编程是互联网编程的基础，Spring为开发者提供了JDBC模板模式，那就是它自身的JdbcTemplate，它可以简化许多代码的编程，但是在实际工作中JdbcTemplate并不常用。

       Spring还提供了TransactionTemplate支持事务的模板，只是这些都不是常用技术，对于持久层，工作中更多的时候用的是Hibernate框架和MyBatis框架。Spring并不去代替已有框架的功能，而是以提供模板的形式给予支持。（这里说得很好，Spring是让已有的好的组件都能很好的接入到Spring框架）

      Spring最重要的功能毫无疑问就是操作数据。JDBC代码有太多的try……catch……finally……语句，导致代码可读性和可维护性。如果你用得好JDBC，其性能最好的，但是太多的try……catch……finally……语句需要处理，数据库资源的打开、关闭都是定性的，甚至大部分情况下，只要发生一场数据库的事物就会回滚，否则就提交，二者都是比较固定的模式。在Spring没有出现之前，许多开发者在JDBC中滥用着try……catch……finally……语句导致代码可读性和可维护性几句下降，从而引发信任问题。为了解决这些问题，Spring提供了自己的方案，那就是JdbcTemplate模板。



      在Spring中大部分都会配置成数据库连接池，既可以使用Spring内部提供的类，也可以使用第三方数据库连接池或者从WEB服务器中通过JDNI获取数据源。

数据库连接池

配置简单的数据库连接池（Spring自带的数据库资源）

配置第三方的数据库连接池（C3P0 DBCP DRUID）

jndi数据库连接池



MapperFactoryBean的配置，

MyBatis的运行只需要提供类似于RoleMapper.java的接口，而无需提供一个实现类。通过学习MyBatis运行原理，可以知道它是由MyBatis体系创建的动态代理对象运行的，所以Spring也没有办法为其生成实现类，所以MyBatis-Spring就提供了MapperFactoryBean

当使用一个Mapper接口的方法，它就会产生一个新的SqlSession，运行完成后就会自动关闭。



第十三章

       本章先讨论Spring数据库的事务应用，然后讨论Spring中最著名的注解之一-----@Transactional。搞清楚注解@Transactional概念不是那么容易的事情，因为这会涉及到数据库的各种概念，为此有必要先从数据库谈起，这样有利于理解它的配置内容。它的隔离级别和传播行为等抽象概念。

       大部分情况下，我们会认为数据库事务要么同时成功，要么同时失败，但是存在着不同的要求。比如银行的信用卡还款，有个跑批量的事务，而这个批量事务又包含了对各个信用卡的还款业务的处理，我们不能因为其中一张卡的事务的失败了，而把所有卡的事务也回滚，这样就会导致因为一个客户的一场，造成多个客户的还款失败，即正常还款的用户，也被认为是不正常还款的，这样会引发严重的金融信誉问题，Spring事务的传播行为带来了比较方便的解决方案。



第十七章 Redia概述

第二十二章 高并发业务

       互联网存在的一些高并发的实例：疯狂抢手机 春运买火车票 抢红包。

       互联网的开发包括Java后台、NoSQL、数据库、限流、CDN、负载均衡等内容，甚至可以说目前并没有权威性的技术和设计，有的只是长期的经验的总结，使用这些技术经验可以有效优化系统，提高系统的并发能力。

       其实请求来到服务器前需要先通过防火墙（firewall），这里的防火墙的准确定义其实没有很好的理解？

       正如没有完美的程序一样，也不会有完美的架构。

负载均衡：

       对业务请求做初步的分析，决定分不分发请求到Web服务器，这就好比一个把控的关卡，常见的分发软件比如Nginx和Apache等反向代理服务器，它们在关卡处可以通过配置禁止一些无效的请求，比如封禁经常作弊的IP地址，也可以使用Lua、C语言联合NoSQL缓存技术进行业务分析，这样就可以初步分析业务，决定是否需要分发到服务器。

      提供路由算法，它可以提供一些负载均衡算法，根据各个服务器的负载能力进行合理分发，每一个Web服务器得到比较均衡的请求，从而降低单个服务器的压力，提高系统的相应能力。

      限流，对于一些高并发时刻，如双十一，新产品上线，需要通过限流来处理，因为可能某个时刻通过上述的算法让有效请求过多到达服务器，使得一些Web服务器或者数据库服务器产生宕机。持续的宕机会造成连续的服务器雪崩。负载均衡器有限流算法，对于请求过多的时刻，可以告知用户系统繁忙，请稍后再试，从而保证系统持续可用。

      系统完成可以在负载均衡器中进行初步鉴别业务请求，使得一些不合理的业务请求在进入Web服务器之前就排除掉，而为了应付复杂的业务，可以把业务存储在NoSQL（往往是Redis）上，通过C语言或者Lua语言进行逻辑判断，它们的性能比Web服务器判断的性能要快速得多，通过这些简单的判断就能够快速发现无效请求，并把它们拍的虎在Web服务器之外，从而降低Web服务器的压力，提高互联网系统的相应速度。

       数据库设计：对于数据库的设计而言，为了得到高性能，可以使用分表或分库技术，从而提高系统的响应能力。

高并发系统的分析和设计

      任何系统都不是独立于业务进行的，真正的系统是为了实现业务而开发的，所以开发高并发网站抢购时，都应该先分析业务需求和实际的场景，在完善这些需求之后才能进入系统开发阶段。没有对业务进行分析就贸然开发系统是开发者大忌。

      对于业务分析，首先分析有效请求和无效请求

系统设计

水平分发（个模块通过RPC相互访问：DubboThrift Hessian）

垂直分发

水平和垂直结合分发

数据库设计

分表分库 -> 优化SQL语句 ->读写分离技术，进行进一步的优化，这样就可以有一台主机负责写业务，一台或者多台备机负责读业务，有助于性能的提高->对于分布式数据库而言，还会有另外一个麻烦，就是事务的一致性，事务的一致性比较复杂，目前流行的有两段提交协议，即XA协议、Paxos协议。

动静分离技术

锁和高并发
