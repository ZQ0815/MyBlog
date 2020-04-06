## AOP
AOP中的概念：
###横切关注点
对哪些方法进行拦截，拦截后怎么处理，这些关注点称之为横切关注点
###切面
类是对物体特征的抽象，切面就是对横切关注点的抽象
###连接点
被拦截到的点，因为Spring只支持方法类型的连接点，所以在Spring中连接点指的就是被拦截到的方法，实际上连接点还可以是字段或者构造器
###切入点
对连接点进行拦截的定义
###通知
所谓通知指的就是指拦截到连接点之后要执行的代码，通知分为前置、后置、异常、最终、环绕通知五类
###目标对象
代理的目标对象
###织入
将切面应用到目标对象并导致代理对象创建的过程
###引入
在不修改代码的前提下，引入可以在运行期为类动态地添加一些方法或字段
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
