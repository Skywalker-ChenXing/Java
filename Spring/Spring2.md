# 1 Scope

在maven的dependency中见过这个词 ：依赖的作用域 test provide runtime compile

组件的作用域：singleton（单例），prototype（原型）

singleton：容器中的组件 始终以单例的形式存在 默认是单例

prototype：每次从容器中取出组件都是一个新的组件 相当于每一次new一个新的

```xml
<bean class="com.cskaoyan.Bean.SingletonBean" scope="singleton"/>
<bean class="com.cskaoyan.Bean.PrototypeBean" scope="prototype"/>
```

单例取出的是同一个实例化的对象

```java
public void mytest4(){
    ApplicationContext application = new ClassPathXmlApplicationContext("application.xml");
    SingletonBean bean = application.getBean(SingletonBean.class);
    SingletonBean bean2 = application.getBean(SingletonBean.class);
    SingletonBean bean3 = application.getBean(SingletonBean.class);
    System.out.println(111);
}
```

bean3 = {SingletonBean@1833} 
bean2 = {SingletonBean@1833} 
bean = {SingletonBean@1833} 

每次都new一个新的对象

```java
public void mytest5(){
    ApplicationContext application = new ClassPathXmlApplicationContext("application.xml");
    PrototypeBean bean = application.getBean(PrototypeBean.class);
    PrototypeBean bean2 = application.getBean(PrototypeBean.class);
    PrototypeBean bean3 = application.getBean(PrototypeBean.class);
    System.out.println(111);
}
```

bean = {PrototypeBean@1834} 
bean2 = {PrototypeBean@1835} 
bean3 = {PrototypeBean@1836} 

# 2 CollectionBean(xml的spring 配置文件)

要在组件中定义一些collection类型的成员变量 --注入值

注册组件的时候，会遇到组件的成员变量类型为 array，list，set等类型，这个成员变量在组件注册过程中怎么写

```xml
<bean id="user" class="com.cskaoyan.Bean.User">
    <property name="username" value="liupinjia"/>
    <property name="password" value="gogo"/>
</bean>
<bean class="com.cskaoyan.Bean.CollectionBean">
    <property name="arrayParam">
        <array>
            <value>arraydata1</value>
            <value>2345</value>
            <ref bean="user"/>
            <!--ref bean = “组件id 引用容器中注册的实例化对象-->
            <bean id="user" class="com.cskaoyan.Bean.User">
                <property name="username" value="chenxing"/>
                <property name="password" value="gogo"/>
            </bean>
        </array>
    </property>
    <property name="listParam">
        <list>
            <value>123</value>
            <value>哈哈哈</value>
            <ref bean = "user"/>
            <bean class="com.cskaoyan.Bean.User">
                <property name="username" value="chen"/>
                <property name="password" value="111"/>
            </bean>
        </list>
    </property>
    <property name="setParam">
        <set>
            <value>hello</value>
            <value>908</value>
            <ref bean="user"/>
            <bean class="com.cskaoyan.Bean.User">
                <property name="username" value="en"/>
                <property name="password" value="1"/>
            </bean>
        </set>
    </property>
</bean>
```

map和properties：map需要key和value：key和value 可以是字符串，基本类型，包装类，javabean properties也需要key和value

key和value不能是javabean

```xml
<property name="mapParam">
    <map>
        <entry key="key1" value="value1"/>
        <entry key-ref="user" value-ref="user"/>
        <entry key="key2"><value>value2</value></entry>
        <entry key="user2"><ref bean="user"/></entry>
        <entry key="user3">
            <bean class="com.cskaoyan.Bean.User">
                <property name="username" value="en"/>
                <property name="password" value="1"/>
            </bean>
        </entry>
    </map>
</property>
<property name="properties">
    <props>
        <prop key="key1">value1</prop>
        <prop key="key2">哈哈</prop>
    </props>
</property>
```



# 3 注解（很重要）

首先要打开注解扫描开关

```xml
<context:component-scan base-package="com.cskaoyan"/>
```

## 3.1 组件注册

注解写在扫描包范围内的类上

@Compoonent 组件

特殊场景的使用

- @Service service层组件
- @Repository dao层组件
- @Controller 控制层组件（SpringMvc 阶段才能使用）

使用注解后组件的id是什么：

可以指定id：@component（"helloService"）使用注解的value属性指定id

可以使用默认id：如果没有指定id，那么使用的是默认id

```xml
<context:component-scan base-package="com.cskaoyan"/>
```



## 3.2注入类

### 3.2.1 字符串 基本类型 包装类（@value）

```java
@Component
public class UserService {
    //使用value注解，不需要包含set方法
    //@Value注解要使用在容器的组件中
    @Value("chenxing")
    String name;
    @Value("15")
    Integer maxsize;
    @Value("3.14")
    double pi;
}
```

value中的值不写死，引用properties配置文件中的key

1. 在spring中引入properties文件
2. 在@value注解中引用key

```xml
<context:property-placeholder location="classpath:param.properties"/>
```

```java
@Component
public class UserService {
    //使用value注解，不需要包含set方法
    //@Value注解要使用在容器的组件中
    @Value("${username}")
    String name;
    @Value("${maxsize}")
    Integer maxsize;
    @Value("${pi}")
    double pi;
}
```

### 3.2.2 javaBean

1.@Autowired ：getBean 容器中这个类型的组件只有一个

2.@Autowired+@Qualifier 容器中这个类型的组件不止一个，通过@Qualifier指定组件id

3.@Resourse 默认可以按照类型去取，也可以使用name属性指定组件id

#### 3.2.2.1 类型组件只有一个

@Autowired

#### 3.2.2.2 类型组件不止一个

@Autowired

@Qualifier

## 3.3 Scope

写在类上

singleton，prototype

```java
@Component//默认id类名的首字母小写 userService
@Scope(Prototype)
public class UserService
```

## 3.4 生命周期

int-method

destory-method

之前是写在bean标签中的属性

```java
@Component
public class LifeCycleBean{
	@PostConstruct
	public void init(){}
	@PreDestroy
	public void destroy(){}
}
```

## 3.5 单元测试

之前单元测试都是先去获得 ApplicationContext对象

Spring对单元测试有良好的支持

可以在单元测试类中直接使用3.2注入类的注解从容器中去除组件

### 3.5.1 引入依赖

```xml
<dependency>
	<groupId>org.springframework</groupId>
	<aftifactId>Spring-test</aftifacatId>
	<version>5.2.6.RELEASE</version>
</dependency>
```

### 3.5.2加载配置文件

在配置类上增加注解

@Runwith

@ContextConfiguration

### 3.5.3 注意事项

这些注解要在组件中使用

不是说所有的类都要到spring容器中注册：一般是提供了某些方法的类注册到容器中。