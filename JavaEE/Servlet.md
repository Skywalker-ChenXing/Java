# 1.  老师介绍

张松。EE阶段、linux。

三周。

前一周多一点：EE阶段核心知识点。非常重要。一定要重视。

项目一。web项目。7个工作日。

还有一些3-4天左右的内容。

# 2.  学习方法

1.晚上会花时间去看视频吗？建议不要去看。

2.做笔记。

建议的学习方式：

首先上课认真听，手里拿着笔，记一些重要的知识点。

课后，积极地去回顾，根据笔记。

敲代码这一步非常重要。（听课可以听得懂，看老师写代码也可以看得懂，但是自己写不出来，代码写少了。）

写代码技巧：

如果能够自己写出来，那最好了；

如果写不出来，看文档去回忆，如果实在不行，

那么看着老师的代码，自己敲一遍。

3.关于提问

大家在提问之前，最好有自己的思考（提问的艺术）。

先自己去尝试解决，带着自己的一个尝试去提问。

# 3.  回顾

EE：Enterprise Edition。java的企业版本。

对于一个企业来说，需要有哪些？

服务器（不是必须的，阿里云）、企业网站（html页面）、数据库DB。

用户如何访问企业网站？

通过域名，发起一个http请求，去请求对应的一个网站资源

对于EE阶段，两个知识点是前置技能要求：

http协议、

tomcat（

对于tomcat的组成部分要有一个认识、

部署应用的几种方式：

1.直接部署-----webapps目录下新建一个目录，那么该目录就是一个应用，目录的名称就是应用名；打成war包丢到webapps目录下，tomcat会自动解压）

2.虚拟映射------非常非常重要。Server.xml文件中再host节点下新增一个Context节点；conf/Catalina/localhost目录下，新增应用名.xml文件。

 

静态资源和动态资源

静态资源：一成不变的内容。每个人看到的都是一样的。Html、css、js、图片等。

动态资源：内容是丰富多彩的。你们刷抖音。兴趣爱好不同，刷到的视频都是不同的。登录一个网站，每个人看到的都是自己的用户名。更多的交互性。Servlet。刷新一个页面，时刻显示最新的时间。

动态资源会转成静态资源，再传输回客户端。

# 4.  Servlet

Servlet = Server + applet 服务器上面的一个小程序。

学习官方文档。

http://tomcat.apache.org/tomcat-8.5-doc/servletapi/overview-summary.html

## 4.1. 如何开发servlet

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image002.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image004.gif)

问题：包不存在？

为什么SE阶段没有遇到过这个问题呢？

SE阶段用的各种API都是位于jdk中的，但是servlet属于EE，ee的包肯定不在SE的jdk中。

导入该包到内存中。

类加载的机制。

类加载器三类：

1.Bootstrap：主要用于加载jdk中的一些核心类库

2.Extension：主要用来加载jre/ext目录下的类库

3.System：将-classpath后面的jar包加载到内存中

 

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image006.gif)

编译通过。

一个class文件如何放入到tomcat中？

tomcat里面只可以放置应用。

需要在tomcat中新建一个应用，然后将class文件放入应用中。

 

EE项目的目录结构：

webapps目录：

一个一个的应用，比如servlet

-----静态资源文件，html、css、js、图片等

-----WEB-INF

----------------classes

----------------lib

----------------web.xml

 

为什么需要这么设置？

假设如果直接将class文件放在servlet应用根目录下，会怎么样？

直接会将你的源代码暴露出来。

WEB-INF目录就是为了屏蔽浏览器直接访问的。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image008.gif)

接下来，怎么能够让tomcat调用该class文件。

也是相当于做了一个映射。Web.xml文件中，配置一个映射信息，当访问/xxx的时候，其实是访问当前servlet的内容。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image010.gif)

 

问题：如何访问该servlet？

访问servlet的话，同样也是要先访问应用，因为servlet属于某一个应用，

如何去访问到一个应用。

## 4.2. 总结servlet执行流程

比如访问http://localhost:8080/app/servlet.

1.地址栏打出该地址，浏览器首先帮助你构建一个请求报文

2.传输到目标机器，到达指定端口8080的connector，然后connector接收到该请求，将请求报文转成request对象

3.connector将request对象，同时还会生成一个空的response对象，然后将这两个对象传给engine

4.engine接着选择host，将这两个对象传给host

5.host选择Context（/app），将这两个对象传给Context

6.Context在当前应用下去寻找/servlet，如果找到，则往response对象里面写入对应的数据

7.这两个对象依次返回，给connector

8.connector读取response对象里面的数据，然后生成一个响应报文，发送出去

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image012.gif)

## 4.3. 开发servlet的第二种方式

IDEA。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image014.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image016.gif)

问题：

访问http://localhost:8080，那么打开的是哪个页面？？？？？

ROOT应用，里面的index三个页面。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image018.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image020.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image022.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image024.gif)

根据配置的应用名，加以访问即可。

## 4.4. 开发servlet的第三种方式

可以继承GenericServlet，也可以继承HttpServlet。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image026.gif)

## 4.5. 开发servlet的第四种方式

之前我们声明servlet都是采用web.xml方式，那么也可以使用注解的方式。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image028.gif)

注解其实还可以进一步简化

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image030.gif)

## 4.6. 编写servlet需要实现service方法，那么为什么继承HttpServlet不需要

因为在HttpServlet中已经做了实现。为什么继承GenericServlet时，重写service，但是继承HttpServlet时，重写doGet或者doPost呢？执行逻辑是什么样的呢？

如果让你去分析，你打算怎么做？你的切入点在哪？

HttpServlet的service方法。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image032.gif)

首先会调用service方法，service方法进行参数向下转型，然后调用HttpServlet自身的service方法，方法里面的逻辑就是根据请求方法的不同，分发到不同的doXXX方法中。

## 4.7. 为什么要区分get和post方法呢？

对于登录，仅允许使用post方法。做更精细的划分。

## 4.8. IDEA如何与tomcat关联

部署应用的方式？两种。直接部署、虚拟映射。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image034.gif)

去看一下这个路径。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image036.gif)

接下来，IDEA会使用复制的这些配置文件，重新开启一个新的tomcat。利用这个新的tomcat来部署应用。

在左边这个目录下conf/Catalina/localhost下，找到了虚拟映射的配置文件。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image038.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image040.gif)

那么docBase指向的位置就是当前app应用的根目录。打开该docBase

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image042.gif)

标准的java web项目目录结构。

但是到此还没有结束，

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image044.gif)

红线框出来的路径不是我们最终的部署路径？？？？

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image046.gif)

对于开发目录和最终部署目录之间的关系？

对于src目录下的java文件来说，首先需要进行编译，编译产物复制到部署目录WEB-INF/classes目录里

对于web目录里面的文件来说，会直接复制到部署的根目录里去。

## 4.9. IDEA项目设置

上面已经提到转换的规则，那么规则是在哪里配置的呢？

右键选择module，选择open module setting

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image048.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image050.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image052.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image054.gif)

 

## 4.10.  servlet的生命周期

servlet的init、service、destroy三个阶段。

servlet被创建的时候，会调用init方法

servlet发挥功能，会调用service方法

servlet将要被销毁，会调用destroy方法

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image056.gif)

Init:servlet在实例化的时候，会调用init来完成一些初始化，但是servlet只会实例化一个对象出来，所以init只会在当前servlet第一次被访问的时候执行。

Service：任何发往servlet的请求，都会执行service方法，也就是doGet或者doPost

Destroy：当前应用被卸载或者tomcat服务器被关闭

另外的一个补充

默认情况下，init会在第一次调用该servlet的时候执行，但是也可以通过添加相应的参数，

在应用被加载的时候，就执行。

设置load-on-startup为非负数即可。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image058.gif)

Web.xml方式同理

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image060.gif)

那么这么做有什么意义呢？

假如某个servlet需要在初始化时需要做一些操作，比如访问数据库等

## 4.11.  Url-pattern细节

1.一个servlet可以设置多个url-pattern吗？

可以。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image062.gif)

2.多个servlet可以设置同一个url-pattern吗？

不可以.

​     Caused by: java.lang.IllegalArgumentException: 名为 [com.cskaoyan.servlet.urlPattern.UrlPatternServlet2]和 [com.cskaoyan.servlet.urlPattern.UrlPatternServlet1] 的servlet不能映射为一个url模式(url-pattern) [/url2]

 

3.url-pattern究竟有哪些写法呢？必须以/开头吗？

不一定，但是url-pattern只有两种写法

一种是/xxxx写法

另外一种*.后缀，比如*.html *.jsp等

最常见的犯错是直接写一个url-pattern叫做servlet1.

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image064.gif)

2和3的报错信息，大家一定要学会找。疫苗----产生抗体。

疑惑：

究竟访问一个html的时候，访问的是静态资源html还是动态资源servlet？

或者换一个问法？

**当访问静态资源html****时，有么有servlet****参与进来**？有的。/

## 4.12.  Url-pattern优先级

Bug-list.

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image066.gif)

总结规律：

1. /开头的url-pattern优先级要高于*.后缀的

2. 如果多个/开头的url-pattern同时满足，那么匹配的程度越高，越优先执行谁

 

## 4.13.  两个特殊的url-pattern

/*  /。

在项目中，不要随意设置/*，因为该url-pattern优先级比较高，可能很多文件无法正常显示。

 

在项目中同时设置/*和/,静态资源文件以及jsp均无法正常显示，把/*去掉，发现jsp可以正常显示，但是静态资源文件仍然无法显示，为什么呢？

其实当你访问index.jsp时，此时仍然访问的是一个servlet。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image068.gif)

当访问http://localhost:8080/index.jsp时，此时执行流程是什么样的？

正常情况下，访问jsp页面，此时是一个叫做jsp的servlet在执行，该servlet的url-pattern叫做*.jsp。

那么，当/*存在时，会发生什么样的情况？

所以，此时永远访问到的是/*

 

为什么当把/*去掉之后，静态资源仍然无法正常显示呢？

此时显示的是/的内容。为什么呢？因为/本身就是用来显示静态资源文件的。如果你在项目中设置了一个/，那么tomcat会认为你有了一个更好的实现，此时，tomcat提供的/的servlet就不会再给你使用了，如果你想显示静态资源文件，需要自己去实现具体的业务逻辑。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image070.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image072.gif)

DefaultServlet是用来处理静态资源文件的。如果一个请求来了以后，发现没有任何的servlet能够匹配到它，那么就会调用default servlet来处理该请求。廉租房的概念。

 

为什么设置/*和/之后，很多页面均无法访问了？

设置/*,优先级比较高，jsp页面。1.txt均无法显示，显示的都是/*里面的内容

当把/*去掉之后，jsp页面可以正常显示（实际上访问的是jsp的servlet，），为什么此时1.txt仍然无法正常显示呢？

当你输入一个/1.txt请求时，发现当前应用下所有的servlet均不能够匹配该请求，将该请求交给default servlet来处理，但是你在自己的应用下重新写了一个default servlet，tomcat默认提供的就不会再给你使用了，使用的是你自己的default servlet。

接下来，框架阶段，SpringMVC，核心分发器DispatcherServlet  url-pattern /。

## 4.14.  ServletConfig

做一个简单的了解即可，重要性不是非常的重要。

可以获取某一个servlet的初始化参数。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image074.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image076.gif)

## 4.15.  ServletContext

非常核心的一个对象。

### 4.15.1.     获取全局性初始化参数

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image078.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image080.gif)

### 4.15.2.     可以作为一个全局性共享数据的场所

假如在某个servlet中处理了一个逻辑，保存了一些数据，接下来，在其他servlet中需要把该数据取出来，继续处理，应该怎么做？某网站的历史访问次数。

数据库。

`Context.setAttribute(key,value)/getAttribute(key)/removeAttribute(key)`

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image082.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image084.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image086.gif)

某网站历史访问次数？

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image088.gif)

 

### 4.15.3.     获取EE项目的绝对路径

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image090.gif)

关于file的相对路径，其实jdk描述的是相对jvm调用目录，哪个目录下调用jvm，那么相对路径相对的就是该目录。

想获取项目部署根目录里面的一个文件，所以这种方法不可行。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image092.gif)

 

 