## AOP
AOP中的概念：
### 关注点
重复的代码
### 切面
关注点形成的类
### 切入点
- 执⾏⽬标对象⽅法，动态植⼊切⾯代码。
- 可以通过切⼊点表达式，指定拦截哪些类的哪些⽅法； 给指定的类在运⾏的时候植⼊切⾯类代
码。
###切入点表达式
```
返回类型		包名.类名.方法名.（参数）
通配：* *.*.*(..)
```
###通知方法类型
```
前置通知：
<aop:before method="beforeMethod" pointcut-ref="addtohello"></aop:before>
环绕通知：
<aop:around method="aroundMethod" pointcut-ref="addtohello"></aop:around>
后置通知：
<aop:after method="afterMethod" pointcut-ref="addtohello"></aop:after>
异常通知：
<aop:after-throwing method 	="exceptionMethod" pointcut-ref="addtohello"></aop:after-throwing>
```


## 配置AOP

### XML配置
```
<!-- 1. 目标类 -->
<bean id="helloworld" class="com.CodeAm.springstudy.AOP.SpringAopDemoOne.HelloWorld"></bean>
<!-- 2. 通知类 -->
<bean id="logger" class="com.CodeAm.springstudy.AOP.SpringAopDemoOne.Logger"></bean>
<!-- 3. aop编程 -->
<aop:config>
    <!-- 配置切面，指定哪些方法需要被代理加强 -->
    <aop:pointcut id="addtohello" expression="execution(* com.CodeAm.springstudy.AOP.SpringAopDemoOne.HelloWorld.*(..))"/>
    <!-- 配置通知，具体加强的内容 -->
    <aop:aspect ref="logger">
        <aop:before method="beforeMethod" pointcut-ref="addtohello"></aop:before>
        <aop:around method="aroundMethod" pointcut-ref="addtohello"></aop:around>
        <aop:after method="afterMethod" pointcut-ref="addtohello"></aop:after>
        <aop:after-throwing method="exceptionMethod" pointcut-ref="addtohello"></aop:after-throwing>
    </aop:aspect>
</aop:config>
```

### 注解配置

```
@Configuration
@ComponentScan(basePackages = "com.CodeAm.springstudy.AOP")
@EnableAspectJAutoProxy：指明xml支持扫描配置aop
@Aspect：某个类作为切面类
@Pointcut("execution()")：指明切入点
@Around("pointMethod()")：指明连接点之间的环绕方法
```


## 参考文献
>[AOP-1](https://blog.csdn.net/dadiyang/article/details/82920139)
