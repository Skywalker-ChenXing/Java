# 1.  JSP

Java Server Pages.java动态页面的规范。tomcat做了实现。

jsp的特点：

1.就可以把jsp当做html来对待，但是它相较于html还有很大的优势

2.这里面可以嵌套java代码，然后显示动态数据

3.java代码不可以随意的书写，需要在特殊的标签里才有效

## 1.1. jsp原理

从本质上来说，jsp也就是servlet。

从访问http://localhost:8080/index.jsp开始分析。

1.当前请求会交给谁？tomcat里面配置了一个servlet，它的url-pattern是*.jsp  *.jspx，所以当前请求会交给它

2.这个servlet其实就是jsp引擎。该引擎做了一个什么事情呢？首先根据你访问的资源index.jsp，到指定目录下去寻找该文件，如果找得到，则将该文件翻译称为java文件(其实就是servlet,形成index_jsp.java文件),之后再经过编译，形成index_jsp.class文件

3.会调用servlet的service方法，然后显示出页面里面的数据

 

### 1.1.1.  既然jsp本质来说就是servlet，那么在哪？

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image002.gif)

该目录就是IDEA复制tomcat配置文件，开启新的tomcat的目录。

查看java文件源码，发现的确就是servlet。

service方法就是调用Writer写出数据到响应报文中。

对于绝大多数的数据，完全是直接包裹起来，写给客户端

只是在jsp一些特殊标签里面，会做一些处理。

### 1.1.2.  既然jsp也是servlet，为什么大费周章地去弄一个jsp

利用各自的长处，各司其职。代码会比较容易进行维护。逻辑代码变更不会影响到页面代码，页面代码需要维护，也不会一不小心影响到逻辑代码。

## 1.2. jsp语法

jsp其实就是将绝大多数的代码原封不动的发送给客户端，让客户端去处理，但是自己只会去处理一部分，这部分其实就是jsp语法。

### 1.2.1.  jsp表达式

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image004.gif)

jsp表达式里面的java代码，会原封不动地被out.print方法包裹起来。最终翻译过后要符合java的语法。

### 1.2.2.  jsp脚本片段

 

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image006.gif)

注释完全可以写

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image008.gif)

jsp脚本片段可以写多个，每一个的话允许不符合java语法，但是最终拼接出来的结果一定要符合java语法。

#### 1.2.2.1.     jsp脚本片段和jsp表达式是否都可以向客户端输出数据

jsp表达式肯定可以输出。脚本片段能不能输出，取决于你掉不掉用response相关代码。

### 1.2.3.  jsp声明

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image010.gif)

### 1.2.4.  jsp注释

是写在主体区域里面的，不能嵌套在jsp表达式、脚本片段、声明这些里面，只能写在页面的主体部分。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image012.gif)

html注释会原封不动的翻译到客户端。

jsp注释，在翻译称为java代码时，就会消失。

## 1.3. jsp九大对象

在service方法内部会创建出来9个对象，可以直接供我们来使用。默认情况下只能看到8个，如果想看到第9个，需要设置isErrorPage=true

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image014.gif)

Request

Response

pageContext

Session

Exception

Application--servletContext

Config

Out

Page

演示session的时候，我把项目完成以后打开页面的勾选去掉了，为什么？和jsp有关系。不让它自动去打开jsp页面。

如果让jsp页面自动打开，那么就会去创建一个session，创建了session，后面再servlet中演示的时候，就会看不到创建session的过程。

 

### 1.3.1.  pageContext对象

九大对象中最为核心的一个对象。通过该对象可以获得其他八个对象。

#### 1.3.1.1.     API

本身也是一个域。Request、session、application。

该域大小是当前页面内。

1.域API

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image016.gif)

2.可以给其他域赋值

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image018.gif)

如果让你来实现，你怎么实现？

setAttribute(key,value,scope){

If(scope ==1){

pageContext.setAttribute(key,value)

}else if(scope == 2){

pageContext.getRequest.setAttribute(key,value)

}

}

3.findAttribute

演示步骤：

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image020.gif)

```
1.首先在当前页面内先不转发，直接打印pageContext.findAttribute("name")
```

显示的是page

2.调用转发，然后在被转发页面打印该语句，显示的是request

3.直接访问被转发页面page.jsp，打印的是session

4.在page.jsp中调用session.invalidate，打印的是application

得出结论：

一级一级去查找。从最小域pageContext域开始，依次request、session、application域去查找，在某个域找到数据则结束，找不到则接着下一个

### 1.3.2.  out对象

输出数据到客户端。带有缓存功能的Writer。

冲洗条件：

1.buffer缓冲区满

2.关闭buffer缓冲区

3.页面需要响应

 

 

Vue。页面解决技术。

## 1.4. 如何servlet debug

1.明确代码执行流程是什么样的？

2.找到servlet的入口，service方法-doGet或者doPost（form method, a标签默认get请求）

3.在方法第一行打一个断点。

4.一步一步调试

5.该看的都看了之后，点击resume program，会直接执行到下一个断点，如果没有下一个断点，，则直接全部执行完毕

# 2.  Listener--记住一个listener即可,过程能够掌握更好

监听器。这个listener是接下来Spring开始的入口。

现实生活中的案例：

监听对象：艺人明星

监听事件：吸毒嫖娼

监听器：朝阳人民群众

触发事件：报警

 

Web：

监听对象：ServletContext

监听事件：context创建和销毁

监听器：自己编写的一个监听器

触发事件：监听器里面的代码执行

 

20年前写好的代码是怎么知道去调用现在编写的一个方法的呢？

## 2.1. 如何去编写一个listener呢？

1.编写一个类实现ServletContextListener接口

2.注册该Listener

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image022.gif)

 

 

## 2.2. 回调案例

员工老板。员工工作工作完成之后汇报给老板。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image024.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image026.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image028.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image030.gif)

 

listener分类：三个域对象创建销毁的listener

# 3.  Filter

过滤器。

## 3.1. filter的功能

1.可以设置拦截或者放行(验证，是否登录，info页面仅登录可用)

2.可以在请求到达servlet之前修改request对象，也可以在响应之后修改response对象（字符编码格式）

## 3.2. 如何编写Filter

1.编写一个类实现javax.servlet.Filter接口

2.注册该filter(先在web.xml中根据提示完成)

## 3.3. Filter的生命周期

1.Init：随着应用的启动而实例化

2.doFilter：每访问一次filter，就会执行该方法一次

3.Destroy：应用的卸载或者服务器关闭

## 3.4. Servlet和filter如何关联在一起

最简单的方式就是通过url-pattern。servlet和filter设置相同的url-pattern。

为什么没有报错？

不是说url-pattern不可以设置相同的嘛？为什么filter和servlet设置同一个url-pattern不会报错？

不可以设置相同的url-pattern是因为针对的是servlet。servlet从功能上来说，是开发动态web资源，做出响应的，如果多个servlet配置了相同url-pattern，究竟应该选择哪个来执行呢？

但是filter功能上来说，和servlet完全不同，filter定位拦截、过滤，而不是做出响应。

更多的是功能上的一个差异。功能上的一个定义。

### 3.4.1.  filter和servlet设置相同url-pattern，servlet代码不执行

为什么呢？

filter默认执行的是拦截操作，如果想要放行代码往下执行，必须要有这句话

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image032.gif)

此时filter和servlet的代码均会被执行到。

### 3.4.2.  filter可以设置/*吗？

可以的。不会存在servlet的那些诸多烦恼。但是filter一般不会设置/。

 

 

 