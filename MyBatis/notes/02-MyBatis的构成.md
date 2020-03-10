# Mybatis的核心类
## SqlSessionFactoryBuilder
>该类会根据xml文件配置信息创建对应的SqlSessionFactory。
## SqlSessionFactory
>一个工厂接口，任务是创建SqlSession，类似于一个JDBC的Connection。
## SqlSession
>一个接口类，根据提供的信息（参数）返回对应的结果（Result）.类似于Connection接口，每次用完都需要关闭。
## 映射器
由java接口和XML文件组成，目的如下：
- 定义参数类型
- 描述缓存
- 描述SQL语句
- 定义查询结果和POJO的映射关系

## 接口与XML文件
- 接口中定义方法名字和返回类型
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.codeam.springmvc.mapper.PersonMapper">
    <select id="getPersonById" parameterType="int" resultType="com.codeam.springmvc.model.Person">
    SELECT * FROM USER WHERE id = #{id}
</select>
</mapper>
```
>namespace: 用来注册Mapper
- 配置文件中指明SQL语句及其参数，返回结果。

```
public interface PersonMapper {
    public Person getPersonById(int id);
}
```

# Mybatis的创建流程
- 第一步创建XML文件，用SqlSessionFactoryBuild读取XML的信息来创建SqlSessionFactory。xml的配置信息会解析到Configuration类对象中。
- 创建SqlSession，使用SqlSession中的接口。
- 创建映射器，及java 接口和XML配置文件。