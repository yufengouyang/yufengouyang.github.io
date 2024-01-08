---
title: SSM框架基本原理
date: 2024-01-08 10:20:39
tags:
- 框架原理
- spring
- mybatis
- springMvc
---

### 一：基本概念

SSM框架是SpringMVC ，Spring和Mybatis框架的整合，是标准的MVC模式，将整个系统划分为四层：View层，Controller层，Service层，Dao层

1. Spring运用IOC和AOP思想实现业务对象管理
2. SpringMVC主要负责请求的转发和视图管理
3. Mybatis封装JDBC作为数据对象的持久化引擎



### 二：基本原理

#### 1. Spring原理

我们平时开发接触最多的估计就是IOC容器，它可以装载bean（也就是我们Java中的类，当然也包括service dao里面的），有了这个机制，我们就不用在每次使用这个类的时候为它初始化，很少看到关键字new。另外Spring的AOP，事务管理等等都是我们经常用到的。

**IOC**: 所谓控制反转就是应用本身不负责依赖对象的创建及维护，依赖对象的创建及维护是由外部容器负责的。这样控制权就由应用转移到了外部容器，控制权的转移就是所谓反转，目的是为了获得更好的扩展性和良好的可维护性。

**AOP**: 为Aspect Oriented  Programming的缩写，意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

#### 2. SpringMVC原理

1. 客户端发送请求到DispacherServlet（分发器）
2. 由DispacherServlet分发器去查询HanderMapping（映射）并找到处理请求的Controller（控制器）
3. Controller调用Service业务逻辑处理后，返回ModelAndView
4. DispacherSerclet查询视图解析器并找到ModelAndView渲染指定的视图
5. 视图负责将结果显示到客户端

{% asset_img 20151118190949363.png %}

#### 3. Mybatis原理

mybatis是对jdbc的封装，它让数据库底层操作变的透明。mybatis的操作都是围绕一个sqlSessionFactory实例展开的。mybatis通过配置文件关联到各实体类的Mapper文件，Mapper文件中配置了每个类对数据库所需进行的sql语句映射。在每次与数据库交互时，通过sqlSessionFactory拿到一个sqlSession，再执行sql命令。
