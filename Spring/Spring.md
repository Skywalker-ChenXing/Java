# 1 Spring介绍

SpringIOC

SpringAOP

Spring事务

IOC实例化类对象，管理类对象

# 2 IOC&DI

## 2.1 概念

Spring 框架其实就是偏基础性的框架，可以去整合其他框架，类似于平台

核心：IOC，DI和AOP

IOC：Inserse of Controll 控制反转

- 控制：实例的生成权
- 反转：由应用程序反转给spring
- 容器：容器是放置实例对象的地方   Spring 容器 和 IOC容器

原先实例我们想用的时候自己new出来；到了Spring阶段，把实例的生成权交给了Spring容器，由Spring容器进行生成并管理，当我们想用的时候就从容器中取出来。

DI：Dependency injection 依赖注入

应用程序（贫穷）——Spring容器（富有）

依赖

- 谁依赖谁？应用程序依赖Spring容器
- 为什么依赖？Spring容器包含了应用程序必须的内容

注入

- 谁注入谁? Spring容器注入给应用程序
- 注入了什么？ 应用程序运行所必须的资源

aop：面向切面编程     aspect oriented programming

oop：面向对象编程 	object oriented programming

声明事务支持：

1.  指定方法增加事务

2.  通过注解的指定方法 

3. 指哪儿打哪儿 

4. 保证使用的是同一个connection

Spring的单元测试 提供了Spring环境

## 2.2 特点

Core technologies：dependency injection，event，resources，i18n，validation，databinding，type conversion，spel，aop

依赖注入，资源，i18n国际化（SpringMvc），校验（SpringMvc），数据绑定，类型转换，SpringExpressionLanguage，Aop

## 2.3 入门案例

### 2.3.1 入门案例1

#### 2.3.2.1 定义一个service

```java
public class HelloService {
    public void sayHello(String name){
        System.out.println("hello" + name);
    }
```

#### 2.3.2.2 定义一个Spring容器

通过Spring配置文件来管理

xml的文件，通常名字叫做application（-xx）.xml

既然是xml文件，文件要满足一定过得约束（schema）

约束怎么来？

1. 复制已有的配置文件的约束
2. 从spring的参考文档上的appendix上复制
3. 通过创建文件模板来使用（推荐）效率高

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!-- bean definitions here -->

</beans>
```

#### 2.3.2.3 将组件交给spring管理

组件：交给spring容器管理的实例，我们称之为组件

注册：在spring的配置文件中如何定义实例

HelloService交给Spring管理

```xml
<!-- bean definitions here -->
<!--组件注册-->
<!--id:是组建在Spring中的唯一标识-->
<!--class:全类名-->
<bean id="helloService" class="com.cskaoyan.service.HelloService"/>
```

#### 2.3.2.4 从容器中取出实例进行使用

```java
/**
 * 入门案例1
 * 通过指定的id 来获取对象
 */
public class IOCTest {
    @Test
    public void mytest(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
        HelloService helloService = ((HelloService) applicationContext.getBean("helloService"));
        helloService.sayHello("  chenxing");
    }
}
```

helloService = {HelloService@1831} 

helloService2 = {HelloService@1831} 

helloService3 = {HelloService@1831} 

helloService4 = {HelloService@1831} 

组件中容器中默认以单例的形式存在

### 2.3.3 入门案例2

也是通过spring容器管理组件

管理多个组件 ：多个组件之间有依赖关系

HelloService HelloDao 在Spring 容器中如何维护多个组件之间的关系

#### 2.3.3.1 导包

#### 2.3.3.2 构建业务代码

```java
public class HelloServiceImpl implements HelloService{
    HelloDao helloDao;
    @Override
    public void sayHello(String name) {
        helloDao.sayHello(name);
    }

    public void setHelloDao(HelloDao helloDao) {
        this.helloDao = helloDao;
    }
```

```java
public class HelloDaoImpl implements HelloDao {
    @Override
    public void sayHello(String name) {
        System.out.println("hello" + name);
    }
```

#### 2.3.3.3 注册组件并维护组件之间的关系

在配置文件中要维护组件之间的依赖关系

```xml
 <bean id = "helloDao" class="com.cskaoyan.dao.HelloDaoImpl"/>
    <bean id = "helloService" class="com.cskaoyan.service.HelloServiceImpl">
        <!--name指向的是helloDaoImpl中的set方法-->
        <!--ref标签指向的配置文件中 id = “”helloDao类对象-->
        <property name="helloDao" ref="helloDao"/>
    </bean>
```

#### 2.3.3.4 使用

```java
@Test
public void mytest(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
    HelloService helloService = ((HelloService) applicationContext.getBean("helloService"));
    helloService.sayHello("  chenxing");
}
```

#### 2.3.3.5 思考问题

Spring在这里有没有带来新的功能

提高了组件的重用频率

## 2.4 ApplicationContext

都是去加载application

ClasspathXmlApplicationContext 加载classpath目录下的配置文件

FileSystemXmlApplicationContext 加载文件系统目录下的配置文件

```java
ApplicationContext applicationContext1 = new FileSystemXmlApplicationContext(fileSystemPath);
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
```

### 2.4.1 BeanFactory

BeanFactory：容器中所有组件都是通过这个Bean生产出来的

BeanFactory ：生产所有组件bean

FactoryBean：XXXFactoryBean，factoryBean 对应的特定的xxx实例

## 2.5 xml文件中注册bean的方式

### 2.5.1 构造方法

#### 2.5.1.1 无参构造（最常用）

因为默认提供的是无参构造，这个是最常用

```XML
<bean id = "noArgBean" class="com.cskaoyan.NoArgsConstructorBean">
    <property name="id" value="1"/>
    <property name="name" value="chenxing"/>
</bean>
<bean id = "hasArgsBean" class="com.cskaoyan.HasArgsConstructorBean">

    <constructor-arg name="id" value="2"/>
    <constructor-arg name="name" value="liupinjia"/>
</bean>
```

### 2.5.2 工厂

通常是整合已有代码的时候，可以使用工厂注册组件

```xml
<bean id = "instanceFactory" class="com.cskaoyan.InstanceFactory" />
<bean id = "animalFromInstanceFactory" factory-bean="instanceFactory" factory-method="create"/>
<!--实例工厂 首先在配置文件中实例化一个工厂对象，然后实例化animal调用该工厂对象的方法-->
<bean id = "animalFromStaticFactory" class="com.cskaoyan.StaticFactory" factory-method="create"/>
```

## 2.6 生命周期

Spring 中 Bean中的 生命周期

