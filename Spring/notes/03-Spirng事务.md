##基于AOP的事务管理

## 事务
一个完整连续的操作。中间中断需要回滚。

## 事务控制
- 编程式事务控制
- 声明式事务控制

## 编程式事务控制
⾃⼰⼿动控制事务，就叫做编程式事务控制。

特点：细粒度的事务控制： 可以对指定的⽅法、指定的⽅法的某⼏⾏添加事务控制（⽐较灵活，
但开发起来⽐较繁琐： 每次都要开启、提交、回滚.)

## 声明式事务控制
Spring提供对事务的控制管理就叫做声明式事务控制

- 事务的管理器类
1. Jdbc技术：DataSourceTransactionManager
2. Hibernate技术：HibernateTransactionManager

## 事务的传播行为
1. Propagation.REQUIRED
如果当前⽅法已经有事务了，加⼊当前⽅法事务
2. Propagation.REQUIRED_NEW
如果当前⽅法有事务了，当前⽅法事务会挂起。始终开启⼀个新
的事务，直到新的事务执⾏完、当前⽅法的事务才开始


## 如何配置事务

方法一：基于xml配置事务
```
<!--配置事务管理-->
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"></property>
</bean>

<!--配置事务类-->
<tx:advice id="txAdvice"  transaction-manager="txManager">
    <tx:attributes>
        <tx:method name="*"/>
    </tx:attributes>
</tx:advice>

<aop:config>
    <aop:pointcut id="ptService"
                  expression="execution(* com.CodeAm.springstudy.AOP.SpirngTransationManager.service.imp.AccountServiceImp.*(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="ptService"/>
</aop:config>
```
基于XML配置的多种方式？

基于注解配置事务：
```
@Transactional：注解某个类中的所有public方法为事务方法，在方法上可以覆盖类的效果
@EnableTransactionManagement：允许通过注解配置事务，aop也可以使用。
```

### 事务实例
```
最基本的基于tx的事务管理demo
https://github.com/africancu/spring3
```