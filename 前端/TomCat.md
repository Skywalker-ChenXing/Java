# 1 Tomcat

## 1.1 服务器

服务器至少需要干什么事情:

​    监听端口

​    分析请求报文

​    构建响应报文

​    承载java对象

## 1.2 现成服务器

JavaEE：企业级开发（ejb jdbc jndi  rmi  xml  Servlet  jsp）



## 1.3  安装Tomcat

避免中文路径

设置 JAVA_HOME (命令行`echo %JAVA_HOME%`检查JAVA_HOME是否存在)

## 1.4 Tomcat使用

Tomcat部署vue项目：

1. 在vue项目里执行 `npm run bulid` 打包出一个dist文件（包含一个html和多个css，js等文件）
2. 把dist里面的内容放到tomcat的webapps下面的root里面，启动Tomcat

## 1.5 Tomcat：主要给java程序用的

实际上，主要部署java war包（java程序经过打包，形成一个war包，放到tomcat的webapps中，让tomcat运行）

让tomcat运行war包的本质目的？

根据我们提供的war（代码）创建出响应的对象

## 1.6 怎么通过Tomcat部署java包

### 1.6.1 Maven

前端：`npm run build`

后端：jar maven

War的目录结构：

Metainf：maven的自动生成目录

WEB-INF：项目内容目录

- Web.xml: war包描述文件
- classes ： java代码的class
- lib：项目依赖的jar包

## 1.7 Tomcat的目录结构

Bin: binary: 二进制文件:  一些tomcat启动关闭相关的文件

Conf: tomcat配置文件

Lib: tomcat依赖的第三方包

Logs: 日志文件, tomcat的日志文件

Temp:  临时文件

Webapps: 服务应用

Work: tomcat的工作目录

## 1.8 Tomcat的组成结构

<img src="C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200618200324293.png" alt="image-20200618200324293" style="zoom:150%;" />

Server: 表示整个tomcat, 控制tomcat开启和关闭, 包含多个service

Service: 表示一个具体的服务, 对外的服务. 

   一个服务包含两部分: 

​	1. Connector: (可以有多个端口监听) 监听端口, 监听到请求之后, 交给container处理 封装request, response

​    2. Container: (一个服务一个)  处理请求，具体表现就是一个engine

Engine: 具体处理请求, 要进一步交给host处理，一个engine可以包含多个host

Host: 站点, 虚拟主机，包含多个context

Context: 具体的某个服务, 某个应用

## 1.9 配置虚拟主机host

<img src="C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200618202257469.png" alt="image-20200618202257469" style="zoom:150%;" />

​				`<Context path="/new" docBase="C:\Users\WLoongLee\Desktop\ccc\aaa" />`

## 1.10 虚拟目录映射

### 1.10.1 第一种: conf下server.xml之中host节点下面

<img src="C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200618202533489.png" alt="image-20200618202533489" style="zoom:150%;" />

### 1.10.2 第二种: conf: 



<img src="C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200618203137153.png" alt="image-20200618203137153" style="zoom:150%;" />

创建一个xml,  给用户使用的应用名, 就是xml的文件名

<img src="C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200618203238501.png" alt="image-20200618203238501" style="zoom:150%;" />

## 1.11 默认端口和页面

我们的电脑., 如果一个请求进来, 它没有带端口, 我们的电脑自动的把这个请求, 交给80端口处理，我们可以让tomcat直接监听80端口, 那么所有不带端口的请求, 都会触发到我们的tomcat中去

<img src="C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200618203413987.png" alt="image-20200618203413987" style="zoom:150%;" />

<img src="C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200618203429252.png" alt="image-20200618203429252" style="zoom:150%;" />

<img src="C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200618203446955.png" alt="image-20200618203446955" style="zoom:150%;" />

## 1.12 浏览器打开一个页面的过程（补充）

<img src="F:\王道作业\作业\前端 后端分离.png" alt="前端 后端分离" style="zoom:150%;" />