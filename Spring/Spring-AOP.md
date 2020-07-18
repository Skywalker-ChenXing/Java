# 1 AOP

给容器中的组件做增强

之前做增强：静态代理，动态代理

OOP: 面向对象编程 继承传入一个对象

AOP：面向切面编程



使用动态代理给一个类生成代理对象

aop是给容器中组件批量生成代理对象  把要增强的方法放到一起 这个范围成为切面

一个同学在学校犯错了，老师叫这个同学家长过来，让这个同学变得更强

aop做的是在开家长会，把所有家长都叫过来，一起变得更强

动态代理的InvocationHander中method.invoke 执行前后都可以做一些事情 汉堡模型

# 2 AOP 编程术语

1. Target：目标类（需要被代理的类）

2. JoinPoint： 连接点 指被代理对象里那些可能会被增强的点（方法）如所有方法（候选的可能被增强候选点）

   InvocationHandler中 invoke 方法中的 method 和 args

3. PointCut：切入点  已经被增强的连接点  **划定增强的范围 -->谁**

4. Advice 通知（具体的增强的代码）。代理对象执行到JoinPoint所做的事情。**在什么时间做什么事情**

5. weaver 织入 是指把advice应用到目标对象来创建新的代理对象的过程

6. Proxy 代理类 动态代理生成的

7. Aspect 切面是切入点和通知的结合（一个线是一条特殊的面）一个切入点和一个通知组成一个特殊的面 **谁在什么时间做什么事情 **

   

# 3 AOP 实战

## 3.1 动态代理（手动方式）

### 3.1.1 jdk动态代理

### 3.1.2 cglib动态代理

Spring也提供了cglib动态代理的包

## 3.2 SpringAOP（半自动）

容器中注册组件 通过aop形式生成一个增强组件

通知

### 3.2.1 注册一个委托类target组件

```java
public class HelloService {
    public void sayHello(String name){
        System.out.println("hello " + name);
    }
}
```

### 3.2.2 通知（按照什么样的方式来增强）

```java
public Object invoke(MethodInvocation methodInvocation) throws Throwable {
    System.out.println("正道的光");
    //proceed 继续进行
    Object proceed = methodInvocation.proceed();
    System.out.println("照在了大地上");
    return proceed;
}
```

### 3.2.3 Proxy增强组件

```xml
<!--将目标生成一个代理组件-->
<!--XXXFactoryBean生成一个特定的XXX组件-->
<bean id = "helloServiceProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
    <!--target-->
    <property name="target" ref="helloService"/>
    <!--在什么时间做什么-->
    <property name="interceptorNames" value="customAdvice"/>
</bean>
```

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:application.xml")
public class MyTest {

    @Autowired
    @Qualifier("helloServiceProxy")
    HelloService helloService;

    @Test
    public void myTest(){
        helloService.sayHello("liupinjia");
    }
}
```



### 3.2.4 思考

这样子来增强好不好？

每一个组件都对应一个ProxyFactoryBean

配置起来繁琐

## 3.3 AspectJ(全自动)

aspect for java

批量增强：批量不是组件 而是组件中的方法 要被增强的方法所在的类要注册在容器中

Pointcut：切入点

### 3.3.1 引入全新依赖

建议使用 groupid 带 org这个依赖

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.5</version>
</dependency>
```

### 3.3.2 切入表达式

批量的划分增强方法

execution（修饰符 返回值 包名.类名.方法名（形参））

**能否省略？能否通配？有没有特殊用法？**

#### 3.3.2.1 修饰符

private，public 这些修饰符

可以省略不写：代表任意修饰符

```xml
<aop:config>
    <!--pointcut id为了方便别人引用他-->
    <!--expression切入点表达式 → 匹配方法 → 指定增强方法-->
    <aop:pointcut id="mypointcut" expression="execution(void com.cskaoyan.service.HelloService.sayHello(String))"/>
    <!--pointcut属性写的是切入点表达式-->
    <!--<aop:advisor advice-ref="增强的组件" pointcut=""/>-->
    <aop:advisor advice-ref="customAdvice" pointcut-ref="mypointcut"/>
</aop:config>
```



#### 3.3.2.2 返回值

能否省略：不能省略

能否通配：可以使用 * 通配

特殊用法：如果返回值是javabean则需要写全类名；如果是基本类型或java.lang包目录下的可以直接写

```xml
<aop:pointcut id="mypointcut" expression="execution(* com.cskaoyan.service.HelloService.sayHello(String))"/>
<!--如果返回值是javabean则需要写全类名；如果是基本类型或java.lang包目录下的可以直接写-->
<aop:pointcut id="mypointcut2" expression="execution(void com.cskaoyan.service.HelloService.sayHello(String))"/>
<aop:pointcut id="mypointcut3" expression="execution(com.cskaoyan.service.User com.cskaoyan.service.HelloService.sayHello(String))"/>
```

#### 3.3.2.3 报名+类名+方法名

能否省略：可以部分省略。除了头和尾不能省略，中间的部分都可以省略，通过..来省略中间的部分

能否通配：可以通配 *。头中间尾都可以使用 *来通配。可以代表一个单词或一个单词的一部分

```xml
<!-- ******包名类名方法名******-->
<!--除了头和尾不能省略，中间的部分都可以省略 → 中间使用..来进行省略-->
 <aop:pointcut id="mypointcut6" expression="execution(com.cskaoyan.bean.User com..returnUser())"/>
 <aop:pointcut id="mypointcut7" expression="execution(com.cskaoyan.bean.User com.cskaoyan.service..returnUser())"/>
 <aop:pointcut id="mypointcut8" expression="execution(* *.cskao*.service..return*())"/>
```

#### 3.3.2.4 形参

能否省略：可以省略不写  无参方法

能否通配：*来通配 通配的单个返回值

特殊用法：返回值类似 如果参数是javabean，则要写全类名；如果是java.lang包目录下的可以直接

```xml
<!--******形参******-->
<!--能否省略：省略代表无参方法 参考mypoincut8-->
<!--能否通配：*来通配 通配的单个返回值-->
 <aop:pointcut id="mypointcut9" expression="execution(* *.cskao*.service..login(*,String,Integer))"/>
 <!--使用..来通配任意数量的任意类型的参数-->
<aop:pointcut id="mypointcut10" expression="execution(* *.cskao*.service..login(..))"/>
 <!--特殊点：返回值类似 如果参数是javabean，则要写全类名；如果是java.lang包目录下可以直接-->
 <aop:pointcut id="mypointcut11" expression="execution(* *.cskao*.service..login(com.cskaoyan.bean.User))"/>
```



### 3.3.3 Advisor(通知自己定义)

引用pointcut和通知

#### 3.3.3.1 通知组件

使用springaop是一样的，同样要实现MethodInterceptor接口，并且注册在容器中

```java
public Object invoke(MethodInvocation methodInvocation) throws Throwable {
    System.out.println("正道的光");
    //proceed 继续进行
    Object proceed = methodInvocation.proceed();
    System.out.println("照在了大地上");
    return proceed;
}
```



#### 3.3.3.2 aspectj中的advisor的配置

<aop 引入aop的约束

1. 复制已有的
2. 官网appendix
3. 创建模板
4. 自己改造

```xml
<aop:config>
<aop:pointcut id expression>
<aop:advisor advice-ref pointcut(-ref)>
<aop:config/>
```



### 3.3.4 aspect(提供通知)

定义pointcut和通知

时间：相对于委托类的方法  基线

#### 3.3.4.1 切面类

通知的方法  做什么事情  把该方法配置为对应的通知

```xml
<aop:config>
    <aop:aspect ref="customAdvice">
        <aop:pointcut id="mypointcut" expression="execution(* com.cskaoyan.service..*(..))"/>
        <aop:before method="mybefore" pointcut-ref="mypointcut"/>
    </aop:aspect>
</aop:config>
```



##### 3.3.4.1.1 Jointpoint

```java
public void mybefore(Joinpoint joinpoint){
    System.out.println(joinpoint.getThis().getClass());
    System.out.println("正道的光");
}
```



##### 3.3.4.1.2 通知的使用



#### 3.3.4.2 使用注解来使用aspect

##### 3.3.4.2.1 开启开关

```xml
<aop:aspectj-autoproxy/>
```

##### 3.3.4.2.2 切面类中使用注解

```java
@Component
@Aspect
public class CustomAdvice  {
@Pointcut("execution(* com.cskaoyan.service..*(..))")
//方法名为切入点的id
public void mypointcut(){}

@Before("mypointcut()")
    public void mybefore(){
        System.out.println("正道的光");
    }
@After("mypointcut()")
    public void after(JoinPoint joinPoint) throws Throwable {
        System.out.println("照在大地上");
    }
@Around("mypointcut()")
    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("around的before");
        //委托类的方法
        Object proceed = proceedingJoinPoint.proceed();
        System.out.println("around的after");
        return proceed;

    }
@AfterReturning(value = "mypointcut()" ,returning = "returnValue")
    public void afterReturning(Object returnValue) throws Throwable {
        System.out.println("返回值为"+ returnValue );
    }
@AfterThrowing(value = "mypointcut()",throwing = "exception")
    public void afterThrowing(Exception exception) throws Throwable {
        System.out.println("抛出的异常" + exception.getMessage());
    }
}
```

#### 3.3.4.3 使用自定义注解来指定增强方法

指哪里打哪里

