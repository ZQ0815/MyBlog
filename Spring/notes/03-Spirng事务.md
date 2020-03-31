##基于AOP的事务管理

## 事务
一个完整连续的操作。中间中断需要回滚。

## 事务的传播级别


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