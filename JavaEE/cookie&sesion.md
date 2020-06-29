# 1.  会话技术

 

日常生活中就是谈话。

A:你吃过了吗

B：我吃了

A：吃的什么

B：面

A：什么面，粗面还是细面

 

网络请求过程中，会话。客户端发起一个请求，到达服务器，接下来又发送了一个请求，又到达服务器，服务器是区分不了客户端的。在服务器眼里，客户端全部都是一样的。

为什么会出现这种情况呢？

因为http协议的无状态性。每次请求之间没有任何区别，服务器无法区分请求之间的区别。

最严重的后果就是购物车里面的数据有哪些、登录的是谁？

但是实际上这些情况并没有发生，原因就是在于存在会话技术，解决了这些问题。

会话技术存在两种

Cookie、session

Set-Cookie、Cookie

## 1.1. Cookie

一小串数据。数据是由服务器生成的，然后在响应的过程中传给客户端（以set-Cookie响应头的形式发送给客户端），客户端及时保存下来，等到下次访问服务器的时候(以Cookie请求头的形式发回给服务端)，就会把这个cookie给携带上。

你们去理发，给了你们一张不记名的卡片，下次去理发，带上该打卡即可。

```java
//生成一个cookie
Cookie cookie = new Cookie("name","lisi");
//响应给客户端，以set—Cookie响应头
//自己完全可以构建一个set—Cookie响应头，但是实际上没有必要
//因为response已经帮我们构建好了
//这行代码的含义其实就是帮助我们构建 set—Cookie响应头
response.addCookie(cookie);
```

第一次只看到了setCookie响应头，此时请求头中没有lisi的cookie

第二次访问改servlet，请求头中有了这个信息

### 1.1.1.  案例1

简单的登录案例。用两个浏览器模拟两个用户。直接在浏览器的窗口将登录的用户名打印出来。

不用之前的转发技术，使用今天的cookie技术来实现。

```java
 protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String username = request.getParameter("username");
        Cookie cookie = new Cookie("username" , username);
        response.addCookie(cookie);
        response.setHeader("refresh","2;url =" + request.getContextPath() + "/info");
    }
```

Info servlet代码：

```java
 //应当从请求头中拿到cookie
        //getHeader去一点一点解析，但是也没有必要
        response.setContentType("text/html;charset=utf-8");
        Cookie[] cookies = request.getCookies();
        if (cookies != null){
            for (Cookie cookie : cookies){
                if ("username".equals(cookie.getName())){
                    response.getWriter().println("欢迎你，" + cookie.getValue());
                }
            }
        }
```

过程：

登录login.html,登录成功之后，写入cookie信息，返回给客户端保存，同时发起一个定时啊刷新，跳转至info页面，客户端就会把cookie请求头给带上，服务端取出key为username的cookie value值。

### 1.1.2.  案例2

打印出上次访问该页面的时间。

```
	Cookie[] cookie = request.getCookies();
	if(cookies != null){
		for(Cookies cookie : cookies){
			if("LastLogin".equals(cookie.getname())){
				String value = cookie.getValue();
				Date date = new Date(Long.parseLong(Value));
				response.getWriter().println("Last login" + date);
			}
		}
	}
	//cookie中的value值不能有空格
	//Cookie cookie = new Cookie("Lastlogin",System.currenTimemills() + "");
	response.addCookie(cookie)
```



### 1.1.3.  cookie的设置

为什么关闭浏览器之后，cookie就消失了呢？

#### 1.1.3.1.     设置MaxAge

默认情况下，没有设置的话，表示负数，含义表示仅存在于浏览器内存中；

设置一个正数，表示可以存活多少秒。这个时候，会存在于硬盘中持续一段时间。

如果设置0呢？表示的是删除cookie。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image014.gif)

#### 1.1.3.2.     设置path

默认情况下，如果没有设置path的话，则当访问当前域名下任意资源时，均会带上该cookie，如果想仅部分路径携带cookie，则可以通过设置path来实现。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image016.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image018.gif)

 

接着访问path2

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image020.gif)

这个时候，如果设置了path，那么删除cookie可以删除吗？

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image022.gif)

此时通过抓包发现，无法成功删除该cookie，原因

如果没有设置path，那么删除cookie时，直接设置maxAge=0即可，但是如果设置了path，那么删除cookie时，必须要和生成cookie时设置相同的path。

过程：

在path1里设置cookie，path是path2，第一次访问path2，理应会带上cookie，关键看第二次访问path2，还会不会带上

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image024.gif)

第一次访问path2

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image026.gif)

第二次访问path2

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image028.gif)

#### 1.1.3.3.     设置domain

设置域名。浏览器有一个大的原则，你不能设置和当前应用无关的域名cookie。比如当前域名是localhost,然后你设置了一个cookie，域名是www.baidu.com。这个时候浏览器是不允许的。

父子域名的规则

Zsquirrel.com 127.0.0.1     ------父域名

Video.zsquirrel.com    127.0.0.1  -------一级子域名

Story.video.zsquirrel.com    127.0.0.1 ------二级子域名

 

如果你在父域名下，设置了一个cookie的domain是zsquirrel.com，那么接下来，下面所有的子域名均可以共享你的这个cookie。

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image030.gif)

设置域名

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image032.gif)

接下来，访问子域名的资源

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image034.gif)

进一步子域名

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image036.gif)

### 1.1.4.  cookie细节

1.cookie的name和value值均是字符串类型

2.cookie它只能存储少量的数据，一般不超过4k

3.默认生成的cookie是会话级别的，仅存在于浏览器内存中，如果想要持久化保存，则需要设置一个MaxAge为正数的值，表示在硬盘存活多久；设置0表示删除cookie，但是需要注意的是，如果该cookie设置了path，那么删除时，必须也设置同样的path，否则无法删除

 

不同浏览器之间可以共享cookie吗？？？？？？？

不可以。

 

如果有一些敏感数据，或者需要存储很大量的数据，这个时候，可以怎么办呢？

Session

## 1.2. Session

服务器给每个浏览器创建了一块区域，专门用来存放数据。只要是一个浏览器的行为，均可以把这些数据存放在这个session中。浏览器和某个sessionn对象做了一个绑定。

你去理发，这个时候给你在它的系统里面录入了一条会员信息，用户名是你的手机号，下次去理发，直接把手机号，它在自己的系统里面搜了一下，发现你的会员信息。

### 1.2.1.  session如何和浏览器关联在一起

session如果利用cookie，能不能实现呢？

cookie如何实现的？利用响应报文中的set-Cookie响应头，以及请求报文中的Cookie请求头，已经搭建好的基础架构。

session可不可以利用cookie来实现呢？可不可以把session的凭证将其放入到cookie中，这个时候cookie带回去以及发送回来的全部都是session的凭证。

 

### 1.2.2.  session的使用

在什么情况下会创建session呢？

通过request.getSession()/getSession(boolean create)这个API来创建session对象

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image038.gif)

有就返回；没有就创建

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image040.gif)

有就返回；没有并且create=true，则创建，create=false，返回null

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image042.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image044.gif)

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image046.gif)

当第一次访问/session时，set-Cookie响应头

第二次访问/session,

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image048.gif)

请求头中包含了Cookie请求头，此时响应头中没有再次响应set-Cookie了

原因在于cookie请求头中返回的JSESSIONID是一个有效的session id，服务器根据id找到了对应的session对象。

 