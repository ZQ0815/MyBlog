# IOC

## 具体思想
IoC不是一种技术，只是一种思想，一个重要的面向对象编程的法则，它能指导我们如何设计出松耦合、更优良的程序。传统应用程序都是由我们在类内部主动创建依赖对象，从而导致类与类之间高耦合，难于测试；有了IoC容器后，把创建和查找依赖对象的控制权交给了容器，由容器进行注入组合对象，所以对象与对象之间是松散耦合，这样也方便测试，利于功能复用，更重要的是使得程序的整个体系结构变得非常灵活。

## 配置容器

### XML配置容器
#### 配置依赖
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd

        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd

        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd"
       default-lazy-init="true">
```

#### 开启注解处理器
```
<context:annotation-config/>
```

#### 开启组件自动扫描;使用annotation自动注册Bean，
```
<context:component-scan base-package="com.jzy">
```

#### 引入属性文件
```
<context:property-placeholder location="classpath:dbconfig.properties"/>
```

#### 引入其他xml文件
```
<import resource="classpath:spring-redis.xml"/>
```

#### 注解配置容器
```
@Configuration： //表示这个Java类充当XML配置文件
@ComponentScan(basePackages = "com.bravo.javaconfig")： //相当于XML中的<context:component-scan>标签，设置需要扫描的包
@ImportResource：("classpath:annotationDemoOne.xml")		//引入某个xml配置文件
@Required: 注释应用于 bean 属性的 setter 方法，它表明受影响的 bean 属性在配置时必须放在 XML 配置文件中，否则容器就会抛出一个 BeanInitializationException 异常。下面显示的是一个使用 @Required 注释的示例。
@Resource：按属性名注入（byName）
@PropertiesResource：用于指定properties文件的位置
@Autowired: 按类型自动注入属性。（byType）
@Qualifier: 可能会有这样一种情况，当你创建多个具有相同类型的 bean 时，并且想要用一个属性只为它们其中的一个进行装配，在这种情况下，你可以使用 @Qualifier 注释和 @Autowired 注释通过指定哪一个真正的 bean 将会被装配来消除混乱。下面显示的是使用 @Qualifier 注释的一个示例.
@Bean： 用在方法上，用于把当前方法的返回值作为Bean对象存入spring的IOC容器中，name：用于指定Bean的id，不写默认是当前方法的名称。
@Component：注册一个Bean默认为dao层
@Repository：注册一个Bean默认为mapper层
@Controller：注册一个Bean默认为Controller层
@Service：注册一个Bean默认在Service层
```