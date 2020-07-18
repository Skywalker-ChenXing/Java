# 1 JdbcTemplate(Jdbc模板)

主要是spring提供的一个jdbc框架，简单 代码中写sql 灵活

主要是看使用spring如何整合其他框架 通过spring注册其他框架的组件

## 1.1 se的代码

### 1.1.1 引入依赖

spring-jdbc

mysql-connector-java

druid

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.6.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.2.6.RELEASE</version>
</dependency>
```

### 1.1.2 se代码

```java
DruidDataSource dataSource = new DruidDataSource();
dataSource.setDriverClassName("com.mysql.jdbc.Driver");
dataSource.setUrl("jdbc:mysql://localhost:3306/22_mall?useUnicode=true&characterEncoding=utf-8");
dataSource.setUsername("root");
dataSource.setPassword("123456");
JdbcTemplate jdbcTemplate = new JdbcTemplate();
//主动new的过程可以使用spring管理起来
jdbcTemplate.setDataSource(dataSource);
//看到set方法要想要property标签的name属性

//jdbcTemplate的api
//根据id查询username
String sql = "select username from 22_mall where id = ? ";
String username = jdbcTemplate.queryForObject(sql,String.class,1);
System.out.println("id为1的username" + username);
```



## 1.2 spring整合

使用注解来注册组件：可否使用@Component注解来注册组件 自己写的类Datasource和JdbcTemplate不是我们自己写的 ，所以我们需要注册

### 1.2.1 引入依赖

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.2.6.RELEASE</version>
</dependency>
  <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.2.6.RELEASE</version>
    </dependency>
```



### 1.2.2 spring配置文件

```xml
<!--dataSource-->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"> 
        <!--new-->
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/22_mall?useUnicode=true&amp;characterEncoding=utf-8"/>
    <property name="username" value="root"/>
    <property name="password" value="123456"/>
    <!--property标签的name属性对应的set方法-->
</bean>
<bean class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"/>
</bean>
```



### 1.2.3 使用jdbcTemplate写一个业务

```java
@Service
public class AcoountServiceImpl implements AccoutService {

    @Autowired
    AccountDao accountDao;

    @Override
    public boolean transfer(Integer fromId, Integer destId, Integer money) {
        Integer fromMoney = accountDao.selectMoneyById(fromId);
        Integer destMoney = accountDao.selectMoneyById(destId);

        fromMoney -= money;//转出人的钱变少了
        destMoney += money;//转入人的钱变多了

        accountDao.updateMoneyById(fromMoney,fromId);
        accountDao.updateMoneyById(destMoney,destId);

        return true;
    }
}
```

```java
@Repository
public class AccountDaoImpl implements AccountDao {

//从容器中取出注册的jdbcTemplate
    @Autowired
    JdbcTemplate jdbcTemplate;

    @Override
    public Integer selectMoneyById(Integer id) {
        String sql = "select money from account where id = ? ";
        Integer money = jdbcTemplate.queryForObject(sql,Integer.class,id);
        return  money;
    }

    @Override
    public Integer updateMoneyById(Integer Money, Integer id) {
        String sql = "update account set money = ? where id = ? ";
        int update = jdbcTemplate.update(sql,Money,id);
        return update;
    }

}
```

```java
   @Autowired
    AccoutService accoutService;
    @org.junit.Test
    public void mytest2(){
    accoutService.transfer(1,2,100);
    }
}
```

## 1.3 @Autowired注解的另一个使用方式

注解放在方法上：在组件初始化的时候，会执行到该方法

```java
@Autowired
//形参从容器中取出对应的组件，如果该类型的组件在容器中只有一个，可以直接取出
//如果容器中的该类型的组件不止一个，可以使用@Qualifier来指定组件id
    public void setJdbcTemplate(@Qualifier("jdbcTemplate") JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }
```



### 1.3.1 jdbcDaoSupport

其实用处不大

```java
public Integer selectMoneyById(Integer id) {
        String sql = "select money from j22_account_t where id = ?";
        JdbcTemplate jdbcTemplate = getJdbcTemplate();
        Integer money = jdbcTemplate.queryForObject(sql, Integer.class, id);
        return money;
    }

```



# 2.TX

## 2.1 回顾

特点：

A：原子性 操作数据库的最小单位，不可分割的部分

C：一致性

I：隔离性

D：持久性

事务并发引起的问题：

脏读：一个事务读取到另一个事务还未提交的数据

不可重复读：一个事务读到另外一个事务 已经提交的数据（修改操作）

幻读：一个事务读到另外一个事务 已经提交的数据（增删操作）

|          | 脏读 | 不可重复读 | 幻读 |
| -------- | ---- | ---------- | ---- |
| 读未提交 | ×    | ×          | ×    |
| 读已提交 | √    | ×          | ×    |
| 可重复读 | √    | √          | ×    |
| 序列化   | √    | √          | √    |

mysql默认的隔离级别是什么：可重复读，但不会导致虚读问题

## 2.2 Spring事务

### 2.2.1 核心概念

  ![image-20200717210531485](C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200717210531485.png)

#### 2.2.1.1 PlatFormTransactionManager（平台事务管理器）

并且这个组件依赖数据源

#### 2.2.1.2 TransactionStatus（事务状态）

事务的状态

底层事务的中间变量

#### 2.2.1.3 TransactionDefinition（事务定义）

##### 2.2.1.3.1 传播行为 Propagation

多个事务操作如何共享事务  包含事务的方法之间产生相互作用

```java
class ServiceA{
	void methodA(){
	}
}
class ServiceB{
	void methodB(){
		serviceA.methodA()
	}
}
```

最常用的是

1. Requied：把多个事务看成是同一个事务操作 一荣俱荣一损俱损 要么一起提交事务，要么一起回滚

2. requires_news：总是发起一个新的事务，methodA作为一个单独的新事务

- 如果methodB发生异常，methodA是否回滚？ B回滚，A不回滚

- 如果methodA发生异常，methodB是否回滚？ AB都回滚

3. nested：以嵌套事务的方式运行

- 如果methodB发生异常，methodA是否回滚？ AB都回滚

- 如果methodA发生异常，methodB是否回滚？ A回滚

### 2.2.2 Spring事务案例

核心DataSourceTransationManager

```xml
<!--TransactionManager-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

#### 2.2.2.1 TransationTemplate（手动实现事务）

```java
  	@Autowired
    TransactionTemplate transactionTemplate;
    @Override
    public boolean transfer(Integer fromId, Integer destId, Integer money) {
        Integer fromMoney = accountDao.selectMoneyById(fromId);
        Integer destMoney = accountDao.selectMoneyById(destId);

        fromMoney -= money;//转出人的钱变少了
        destMoney += money;//转入人的钱变多了

        Integer finalFromMoney = fromMoney;
        Integer finalDestMoney = destMoney;

        Integer execute = transactionTemplate.execute(new TransactionCallback<Integer>() {
            @Override
            public Integer doInTransaction(TransactionStatus transactionStatus) {
                Integer update1 = accountDao.updateMoneyById(finalFromMoney, fromId);
                Integer update2 = accountDao.updateMoneyById(finalDestMoney, destId);

                return update1 + update2;
            }
        });

        System.out.println(execute);

        return true;
    }
```

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
<!--TransactionTemplate-->
<bean class="org.springframework.transaction.support.TransactionTemplate">
    <property name="transactionManager" ref="transactionManager"/>
</bean>
```

#### 2.2.2.2 事务代理对象

类似于SpringAOP，通过委托类组件生成代理组件

```xml
<bean id="accountServiceProxy" class="org.springframework.transaction.interceptor.TransactionProxyFactoryBean">
    <!--target 要去增强的组件-->
    <property name="target" ref="accountDaoImpl"/>
    <!--TransactionManager-->
    <property name="transactionManager" ref="transactionManager"/>
    <!--TransactionAttributes-->
    <property name="transactionAttributes">
    <props>
        <!--key是方法名 value是对事务的定义-->
        <!--
            传播行为：PROPAGATION_XXX
            隔离级别:ISOLATION_XXX REPEATABLE_READ DEFAULT
            只读：readonly
            超时：timeout_数字 单位是秒
            rollbackFor：-XXXException
            noRollbackFor:+XXXException
           
            -->
    </props>        
    </property>
</bean>
```

#### 2.2.2.3 事务通知

aspectj的advisor

##### 2.2.2.3.1 导包

aspectjweaver

```xml
<aop:config>
    <aop:pointcut id="txPointcut" expression="execution(* com.cskaoyan.service..*(..))"/>
    <aop:advisor advice-ref="transactionAdvice" pointcut-ref="txPointcut"/>
</aop:config>

<tx:advice id="transactionAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <!--name是方法名-->
        <!--definition在tx：method标签的属性中-->
        <tx:method name="transfer" isolation="DEFAULT" propagation="REQUIRED"/>
    </tx:attributes>
</tx:advice>
```



#### 2.2.2.4 声明式事务 注解（最简单，最方便，最好用，最重要）

注解写在哪里，哪里就增加事务

首先要打开注解开关

```java
@Transactional(propagation = Propagation.REQUIRED,isolation = Isolation.DEFAULT)
```

# 3 Javaconfig

Spring组件的配置

之前的spring配置文件是xml，后续主流技术springBoot 推荐大家使用javaConfig注册组件

配置类 class 注册组件

## 3.1 配置类

```java
 /**
  * @Bean:value属性指定组件id
  * 返回值：可以写组件的class或者接口
  * 方法名：如果没有指定组件id，使用方法名作为组件id
  */
 @Bean("dataSource")
 public DruidDataSource druidDataSource(){
     //在方法体里面，使用se的代码形式完成一个类对象的实例化
     DruidDataSource dataSource = new DruidDataSource();
     dataSource.setDriverClassName("com.mysql.jdbc.Driver");
     dataSource.setUrl("jdbc:mysql://localhost:3306/22_mall?useUnicode=true&characterEncoding=utf-8");
     dataSource.setUsername("root");
     dataSource.setPassword("123456");
     return dataSource;
 }
 /**
  * 形参：会从容器中取出对应的组件
  * 如果该类型组件只有一个，可以直接取出
  * 如果该类型的组件不止一个，需要使用@Qualifier
  */
 @Bean
 public DataSourceTransactionManager dataSourceTransactionManager(@Qualifier("dataSource")DataSource dataSource){
     DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
     transactionManager.setDataSource(dataSource);
     return transactionManager;
 }

@Bean
public JdbcTemplate jdbcTemplate(@Qualifier("dataSource")DataSource dataSource){
    JdbcTemplate jdbcTemplate = new JdbcTemplate();
    jdbcTemplate.setDataSource(dataSource);
    return jdbcTemplate;
}
```