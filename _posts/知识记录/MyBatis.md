title: "MyBatis"
date: 2015-04-10 16:01:32
tags:
- java
- MyBatis
---
![mybatis-logo][2]

#### 首先使用MySql（MariaDB）测试
√测试成功

## 什么是 MyBatis ？ 
MyBatis 是支持**定制化 SQL**、**存储过程**以及**高级映射**的优秀的持久层框架。MyBatis 避免了几乎所有的 JDBC 代码和手工设置参数以及抽取结果集。MyBatis 使用简单的 **XML** 或**注解**来配置和映射基本体，将接口和 Java 的 POJOs(Plain Old Java Objects,普通的 Java对象)映射成数据库中的记录。

## 入门
###安装
想要使用 MyBatis 只需将 mybatis-x.x.x.jar 文件置于 classpath 中。

如果使用 Maven 构建项目，则需将下面的 dependency 置于 pom.xml 中：

	<dependency>
	  <groupId>org.mybatis</groupId>
	  <artifactId>mybatis</artifactId>
	  <version>x.x.x</version>
	</dependency>

## 探究已映射的 SQL 语句
这里给出一个基于 XML 映射语句的示例，它应该可以满足上述示例中 SqlSession 的调用。

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper
	  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<mapper namespace="org.mybatis.example.BlogMapper">
	  <select id="selectBlog" resultType="Blog">
	    select * from Blog where id = #{id}
	  </select>
	</mapper>

插一句：

	public interface SqlSession

这个接口设计的太好了，如下

	<T> T selectOne(String statement, Object parameter);

根据接收类型选择返回类型。

好，回来，看看里边的其中一条语句：
	
	<mapper namespace="org.mybatis.example.BlogMapper">
	  <select id="selectBlog" resultType="Blog">
	    select * from Blog where id = #{id}
	  </select>
	</mapper>

这个剩余部分具有很好的自解释性。在命名空间“com.mybatis.example.BlogMapper”中定义了一个名为“`selectBlog`”的映射语句，这样它就允许你使用指定的完全限定名“org.mybatis.example.BlogMapper.selectBlog”来调用映射语句，就像上面的例子中做的那样：

	Blog blog = (Blog) session.selectOne("org.mybatis.example.BlogMapper.selectBlog", 101);


**另一种办法**

你可能注意到这和使用完全限定名调用 Java 对象的方法是相似的，之所以这样做是有原因的。这个命名可以直接映射到在命名空间中同名的 Mapper 类，并在已映射的 select 语句中的名字、参数和返回类型匹配成方法。这样你就可以向上面那样很容易地调用这个对应 Mapper 接口的方法。不过让我们再看一遍下面的例子：

	BlogMapper mapper = session.getMapper(BlogMapper.class);
	Blog blog = mapper.selectBlog(101);

第二种方法有很多优势，首先它不是基于字符串常量的，就会更安全；其次，如果你的 IDE 有**代码补全**功能，那么你可以在有了已映射的 SQL 语句之后利用这个功能。

对于像 BlogMapper 这样的映射器类（Mapper class）来说，还有另一招来处理映射。它们的映射的语句可以不需要用 XML 来做，取而代之的可以是 Java 注解。比如，上面的 XML 示例可被替换如下：

	package org.mybatis.example;
	public interface BlogMapper {
	  @Select("SELECT * FROM blog WHERE id = #{id}")
	  Blog selectBlog(int id);
	}

**对于简单语句来说，注解使代码显得更加简洁，然而 Java 注解对于稍微复杂的语句就会力不从心并且会显得更加混乱。**因此，如果你需要做很复杂的事情，那么最好使用 XML 来映射语句。

## 范围（Scope）和生命周期
**`提示` 对象的生命周期和依赖注入框架**

依赖注入框架可以创建线程安全的、基于事务的 SqlSession 和映射器（mapper）并将它们直接注入到你的 bean 中，因此可以直接忽略它们的生命周期。如果对如何通过依赖注入框架来使用 MyBatis 感兴趣可以研究一下` MyBatis-Spring `或 MyBatis-Guice 两个子项目。

###SqlSessionFactoryBuilder

这个类可以被实例化、使用和丢弃，一旦创建了 SqlSessionFactory，就不再需要它了。因此 SqlSessionFactoryBuilder 实例的最佳范围是方法范围（也就是本地方法变量）。你可以重用 SqlSessionFactoryBuilder 来创建多个 SqlSessionFactory 实例，但是最好还是不要让其一直存在以保证所有的 XML 解析资源开放给更重要的事情。

###SqlSessionFactory

SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，没有任何理由对它进行清除或重建。使用 SqlSessionFactory 的最佳实践是在应用运行期间不要重复创建多次，多次重建 SqlSessionFactory 被视为一种代码“坏味道（**bad smell**）”。因此 SqlSessionFactory 的最佳范围是应用范围。有很多方法可以做到，最简单的就是使用**单例模式**或者静态单例模式。

###SqlSession

每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的范围是请求或方法范围。绝对不能将 SqlSession 实例的引用放在一个类的静态域，甚至一个类的实例域也不行。也绝不能将 SqlSession 实例的引用放在任何类型的管理范围中，比如 Serlvet 架构中的 HttpSession。如果你现在正在使用一种 Web 框架，要考虑 SqlSession 放在一个和 HTTP 请求对象相似的范围中。
>**换句话说，每次收到的 HTTP 请求，就可以打开一个 SqlSession；返回一个响应，就关闭它。**

这个关闭操作是很重要的，你应该把这个关闭操作放到 finally 块中以确保每次都能执行关闭。下面的示例就是一个确保 SqlSession 关闭的标准模式：

	SqlSession session = sqlSessionFactory.openSession();
	try {
	  // do work
	} finally {
	  session.close();
	}

你应该在你的所有的代码中**一致性地使用这种模式**来保证所有数据库资源都能被正确地关闭。

## 问题

- MyBatis运行流程
- MyBatis缓存机制

## 参考资料

[Mybatis原理分析一 从JDBC到Mybaits ][3]http://sishuok.com/forum/blogPost/list/3899.html



  [2]: /images/mybatis-logo.png
  [3]: http://sishuok.com/forum/blogPost/list/3899.html