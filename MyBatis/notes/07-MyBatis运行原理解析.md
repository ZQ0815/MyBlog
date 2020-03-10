# MyBatis技术难点分析
主要设计的技术难点：
- 反射
- JDK动态代理
- CHLIB动态代理

# SqlSessionFactory构建过程
>SqlSessionFactory是MyBatis的核心类之一，其最重要的功能就是提供创建MyBatis的核心接口SqlSession。

SqlSessionFactory创建步骤如下：
- 第一步，通过XMLConfiguration解析配置的XML文件，将信息存入Configuration中。
- 第二部，使用Confinguration对象创建SqlSessionFactory。SqlSessionFactory是一个接口，它有一个默认的实现类DefaultSqlSessionFactory。

## 构建XMLConfiguration
在SqlSessionFactory构建中，Configuration是最重要的，它的作用如下：
- 读入配置文件。
- 初始化基础配置，别名、插件、映射器、ObjectFactory、typeHandle对象。
- 提供单例。
- 执行一些重要的对象方法，初始化配置信息。

## 映射器的内部组成
>映射器的作用是：将java中传入的参数映射到sql语句中，并将sql返回的结果映射到POJO对象中去。
一般而言，一个映射器由3个部分组成：
- MappedStatement：保存映射节点。包括配置的sql信息，id，resultMap、parameterType等重要配置内容。
- SqlSource：用于提供BoundSql对象。
- BoundSql：建立SQL和参数的地方。有三个常用属性：Sql、parameterObject、parameterMappings

对BoundSql中的三个属性进行具体说明：
- parameterObject：是参数本身，传递简单对象-》自动装箱（int - Integer）、POJO -》POJO、传递多个参数时，会变成一个Map<String, Object>对象，键值的关系如{"1":p1..."param1":p1}, 当使用@Param注解，@Param("key1")则为{"key1":p1..."param1":p1}。可以通过#{param1} 或者#{key1}等去获取。

- parameterMappings：一个parameterMapping对象的list。这个对象描述参数：属性、名称、javaType等重要信息。

- sql属性就是写在映射器里面的SQL。

## 一行代码构建SqlSessionFactory
```
SqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

# SqlSession运行过程
>SqlSession是一个接口，通过SqlSessionFactory可以轻易拿到SQLSession。SqlSession给出了查询、插入、更新、删除的方法。之前的版本中会使用直接使用SqlSession中的方法。而目前，则是使用Mapper映射。Mapper是一个接口，通过使用动态代理的方法，生成MapperMethod对象，然后执行execute方法，再把SQLSession和运行参数传进去。

## SqlSession下的四大对象
>我们已经知道映射器其实就是一个动态代理对象，进入到了MapperMethod的execute方法。然后经过简单的判断就进入SqlSession的四个方法。下面分析这四个方法的具体执行过程。

Mapper的执行过程是通过Executor、StatementHandle、ParameterHandle和ResultHandle来完成数据的操作和结果返回。

- Executor：代表执行器，由它调度其他三个对象。
- StatementHandle：作用是使数据库的PreparedStatement执行。
- ParameterHandle：用于SQL对参数的处理。
- ResultHandle：进行最后数据集的封装返回处理。

## 执行器
>执行器是一个真正执行java操作和数据库交互的东西。在它执行之前可以配置插件。
执行器的种类：
- SIMPLE：简易执行器，不配置就是默认执行器。
- REUSE：是一种执行器重用与处理语句。
- BATCH：执行器重用语句和批量更新，针对批量专用的执行器。

这里以第一种执行器继续讲解执行的过程，
```
1.生成执行器
newExecutor方法中：executor = new BatchExecutor(this, transaction)
2.执行配置插件
interceptorChain.pluginAll(executor)
3.调用StatementHandle
4.使用prepareStatement对SQL编译并对参数进行初始化，prepareStatement是通过StatementHandle中的prepare进行预编译和基础设置。然后通过StatementHandle的parameterize()来设置参数并执行，ResultHandle在组装查询结果并返回给调用者完成一次查询。
```

## StatementHandle数据库会话器
>专门处理数据库会话的。
通过RoutingStatementHandle创建，并通过适配模式找到对应的类型。分为以下三种：
- SimpleStatementHandle
- preparedStatementHandle
- CallableStatementHandle

以第一个为例，分析执行查询的过程：其中主要的三个方法prepare、parameterize、query
```
1.prepare: 对SQL进行预编译。同事设置一些基本运行参数，比如超时，获取的最大行数。
2.parameterize：设置参数，调用ParameterHandle设置参数。
3.query: 执行sql语句返回结果，之后ResultSetHandle对结果进行封装和返回。
```

## ParameterHandle参数处理器
>在StatementHandle的parameterize方法中被调用，用于对预编译语句进行参数设置。
主要的方法：
```
1.getParameterObject：返回参数对象
2.setParameter：设置SQL的语句参数
```

## ResultSetHandle结果处理器
>默认的实现类涉及使用JAVASSIST或者CGLIB作为延迟加载
```
1.默认通过DefaultResultSetHandle类，处理。主要实现的方法
- handleResultSets(Statement)
- handleOutputParameters()
```

# SqlSession运行总结
SqlSession是通过Executor创建StatementHandle来运行的，而StatementHandle要经过以下三步：
- prepared 预编译SQL
- parameterize 设置参数
- query 执行SQL
其中parameterize是通过调用parameterHandle的方法去设置的，而参数是根据类型处理器typeHandle去处理的。query方法是通过resultHandle进行处理结果的封装，先通过typeHandle处理结果类型，然后用ObjectFactory提供的规则组装对象。


