# 1.设计模式

设计模式就是好用的经验

城市包围农村：个别人和团队提出来的，大家纷纷表示好用

不用乱用和滥用：写代码不一定非要使用上对应的设计模式

设计模式不是万能的：不是使用设计模式就可以解决任意问题

## 1.1 软件设计的原则

- S：single 单一职责原则
- O：开放Open封闭原则；对新增代码开放，对修改代码封闭
- L：里氏替代原则 用父类来接收子类实例
- I：isolation 接口隔离原则：不同的功能放到不同的接口中
- D：依赖倒置原则：抽象和具体之间的关系

## 1.2 单例

从应用程序中取出的实例始终是同一个——孤本

节约资源

### 1.2.1 如何查看是否是同一个实例

```java
@Test
public void mytest1(){
	UserService userService = new UserService();
	UserService userService2 = new UserService();
	UserService userService3 = new UserService();
	
	//如何观察实例是否为同一个呢？比较内存地址是否一致
	Syetem.out.println(userService == userService2);
	
	Assert.assertEquals(userService,userService2);
}
```

### 1.2.2 单例的特点

不允许外面调用new 方法 ：实际上调用的是构造方法

1. 构造方法私有

2. 需要提供一个方法给别人调用static，来获得对应单例的实例

3. 类中包含成员变量为它自身

```java
public class Singleton{
	//构造方法私有
	private Singleton(){}
	//自己提供实例
	static Singleton singleton;
	//提供静态方法给其他应用来调用来获得该单例的实例
	public static Singleton getInstance(){
	if(singleton == null){
		singleton = new Singleton();
	}
	return singleton;
	}
}
```

### 1.2.3 入门案例

#### 1.2.3.1 线程不安全的懒加载

```java
public class Singleton{
	//构造方法私有
	private Singleton(){}
	//自己提供实例
	static Singleton singleton;
	//提供静态方法给其他应用来调用来获得该单例的实例
	public static Singleton getInstance(){
	if(singleton == null){
		singleton = new Singleton();
	}
	return singleton;
	}
}
```

#### 1.2.3.2 线程安全的懒加载

```java
public class Singleton{
	//构造方法私有
	private Singleton(){}
	//自己提供实例
	static Singleton singleton;
	//提供静态方法给其他应用来调用来获得该单例的实例
	public synchronized static Singleton getInstance(){
	if(singleton == null){
		singleton = new Singleton();
	}
	return singleton;
	}
}
```

但是加锁会影响效率

#### 1.2.3.3 懒加载和立即加载

懒汉模式和饿汉模式

lazy loading

都是要去获得一个singleton的实例，getlnstance方法来获得

1. 懒加载，在调用getInstance方法的时候才初始化实例
2. 立即加载，在还未调用getInstance方法的时候就已经完成了实例的初始化

#### 1.2.3.4 立即加载

```java
public class Singleton{
	private Singleton(){};
	static Singleton singleton = new Singleton();
    public static Singleton getInstance(){
        return singleton
    }   
}
```

#### 1.2.3.5 立即加载2

```java
public class Singleton{
	private Singleton(){};
	static Singleton singleton;
	static {
		singleton = new Singleton
	}
    public static Singleton getInstance(){
        return singleton
    }   
}
```

#### 1.2.3.6 静态内部类

线程安全的懒加载，还没有效率问题

```java
public class Singleton{
	private Singleton(){};
	static class Inner{
		static Singleton singleton = new Singleton();
		public static Singleton provideSingleton(){
			return singleton;
		}
	}
	public static Singleton getInstance(){
		return Inner.provideSingleton();
	}
}
```

## 1.3 工厂 Factory

去生产实例，工厂要提供create方法。

通过工厂可以吧实例的实例化细节隐藏起来

XXXFactory 的时候，就是获得XXX实例的，当看到类型是XXXFactory的时候，就要意识到这是一个工厂的设计模式

### 1.3.1 简单工厂

通过给工厂的生产方法传入不同的内容，工厂的生产的内容就有所区别

```java
public class AnimalFactory{
	public Animal create(String name){
	   Animal animal = null;
	   if("bull".equals(name)){
	   	animal = new Bull();
	   }else if("pig".equals(name)){
	   	animal = new Pig();
	   }else if ("rabbit".equals(name)){
	   	animal = new Mouse();
	   }
	   return animal;
	}
}
//新增代码 开闭原则无法遵守：对新增开放，对修改封闭
```

### 1.3.2  工厂方法

新增代码来解决上述问题

```java
public interface CarFactory{
	public Car createCar();
	//当需要新的生产实例的时候，新增对应的工厂类即可
}
```

```java
public class BydCarFactory implements CarFactroy{
	@override
	public Car createCar(){
		return new BydCar();
	}
}
```

```java
public class AudiCarFactory implements CarFactroy{
	@override
	public Car createCar(){
		return new AudiCar();
	}
}
```

```java
public class BmwCarFactory implements CarFactroy{
	@override
	public Car createCar(){
		return new BmwCar();
	}
}
```

通过定义工厂接口，由具体的工厂生产具体的实例

工厂都具备create生产方法，但是方法的具体实现是不同的

### 1.3.3 工厂的使用

```java
public class DreamOneCarFactory implements CarFactory{
	@override
	public Car createCar(){
	return null;
	}
	public static Car staticFactoryCreate(){
		return new DreamOneCar();
	}
}
```

```java
@Test
public void mytest(){
	Car car = DreamOneCarFactory.staticFactoryCreate();
}
//静态工厂
```

## 1.4代理

中介

科学上网

### 1.4.1 静态代理

委托类对象：房东

代理类对象：中介

```java
public class HouseOwner{

	public boolean renHouse(Integer money){
		if(money >= 1500){
		return ture;
	} 
	return false;
}
```

```java
public class HouseProxy{
	HouseOwner houseOwner = new HouseOwner();
	
	public boolean rentHouse(Integer money){
		//做增强
		money = money - 500;
		return houseOwner.rentHouse(money);
	}
}
```

```java
public class HouseProxy extends HouseOwner{
	public boolean rentHouse(Integer money){
		//做增强
		money = money - 500;
		return super.rentHouse(money);
	}
}
```

通过代理对象去执行方法，则会被增强

### 1.4.2 动态代理

动态代理，不需要自己定义代理类

#### 1.4.2.1 jdk动态代理

jdk 提供的api来获得代理对象（增强对象）

jdk动态代理使用要有接口的实现

```java
CarFactory carFactory = new AudiFactory();
Car car = carFactory.createCar();
//这里只能用 Car这个接口来接收
Car carProxy = (Car) Proxy.newProxyInstance(Audi.class.getClassLoader(), car.getClass().getInterfaces(), new InvocationHandler() {
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        long start = System.currentTimeMillis();
        Object invoke = method.invoke(car, args);
        long  end  = System.currentTimeMillis();
        System.out.println(end - start + "S");
        return invoke;
    }
});
carProxy.assembly();
```

接口中的方法全部被增强了。

事务：service,service中的每一个方法都要增加事务

#### 1.4.2.2 cglib动态代理

不需要有接口的实现，cglib基于继承去实现的，proxy对象是继承委托类对象，代码写起来基本是一样，获得代理对象的过程使用api不同

需要导包

```java
CarFactory carFactory = new AudiFactory();
Car car = carFactory.createCar();
Car carProxy = (Car) Proxy.newProxyInstance(Audi.class.getClassLoader(), car.getClass().getInterfaces(), new InvocationHandler() {
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        long start = System.currentTimeMillis();
        Object invoke = method.invoke(car, args);
        long  end  = System.currentTimeMillis();
        System.out.println(end - start + "S");
        return invoke;
    }
});
carProxy.assembly();
```

## 1.5 建造者bulider

更侧重于设置参数的过程

在bulider中 在创建bulider时（生产之前）已经完成了实例化

需要保证时钟对同一个实例设置参数，实例就是要求全局变量

set方法如果想要连续使用起来，要求这些set方法的返回值为bulider

.setHeadIq(120).setHeadEq(120).setLegLength()