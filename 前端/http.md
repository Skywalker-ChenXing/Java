# 1.http

## 1.1 网络三要素

url: 指示资源路径

html：承载资源

http：规定资源在网络中的传输方式，超文本传输协议

## 1.2 电脑的交互

比特流： 数字信号，模拟信号，光信号

我们可以近似的认为我们两台电脑交互，网线中是010101

图像--转换为（必须按规定转化）----转化（要按规定转化）--图像

协议：http是一个应用层协议

应用层协议：http ftp dns 

tcp：可靠传输，面向连接，有链接，硬链接

udp：不可靠出书，无连接，软连接，尽最大能力交付

http协议是基于下层的tcp协议的

网络层：ip协议

## 1.3 http 请求

第一步：浏览器调用发送请求-- http

第二步：域名解析：

1. 首先看浏览器有没有缓存，有没有域名所对应的ip
2. 检测操作系统的缓存
3. 检查hosts文件，有没有配置
4. 请求域名服务器:递归，迭代

第三步：建立tcp

三次握手

四次挥手

第四步：发起请求http请求

第五步：响应http请求

## 1.4 http事务

发送http请求和最后获得一个服务器的返回值：整个流程是一个http请求

## 1.5 关注http请求

我们调用浏览器发送请求，在网线中转化为010101在转换过程中，http协议是把一个请求封装成

报文格式，交给下一层协议处理

## 1.6 请求报文

直观看到的是？一个特殊格式的数据

我们要关注的http，报文，所关心的核心，就是这个报文格式

一个http协议，分为两部分，一个请求一个响应

直观的看到的是? 一个特殊格式的数据

我们要关注的http, 报文 ,所关注的核心, 就是这个报文格式

一个http协议, 分为两部分, 一个请求 一个响应
`GET http://www.cskaoyan.com/forum.php HTTP/1.1
Host: www.cskaoyan.com
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: __cfduid=d7832dfca92da4e7e6283d971d4eba2781567604862; Hm_lvt_68c778cd43f08435bd12c4ee37790e40=1568274769,1568512531,1569381548,1569578185; Hm_lvt_2504f1c08c3a31e74bbfb16ecaff376b=1586697147; cZBD_2132_st_p=0%7C1587979476%7C03b71ad5024f5f78f6145c8ae88da9e7; cZBD_2132_viewid=tid_647945; Hm_lvt_2504f1c08c3a31e74bbfb16ecaff376b=1586697147; Hm_lpvt_2504f1c08c3a31e74bbfb16ecaff376b=1588216271; Hm_lpvt_2504f1c08c3a31e74bbfb16ecaff376b=1588218868`



`HTTP/1.1 200 OK
Date: Wed, 17 Jun 2020 03:46:08 GMT
Server: Apache/2.4.18 (Ubuntu)
Set-Cookie: cZBD_2132_saltkey=ZWiWkKY5; expires=Fri, 17-Jul-2020 03:46:08 GMT; Max-Age=2592000; path=/; HttpOnly
Set-Cookie: cZBD_2132_lastvisit=1592361968; expires=Fri, 17-Jul-2020 03:46:08 GMT; Max-Age=2592000; path=/
Set-Cookie: cZBD_2132_sid=oXhOOW; expires=Thu, 18-Jun-2020 03:46:08 GMT; Max-Age=86400; path=/
Set-Cookie: cZBD_2132_lastact=1592365568%09forum.php%09; expires=Thu, 18-Jun-2020 03:46:08 GMT; Max-Age=86400; path=/
Vary: Accept-Encoding
Content-Encoding: gzip
Content-Length: 7440
Keep-Alive: timeout=5, max=99
Connection: Keep-Alive
Content-Type: text/html; charset=gbk`

<img src="https://pic002.cnblogs.com/images/2012/426620/2012072810301161.png" alt="img" style="zoom:150%;" />

### 1.6.1 请求行

`GET http://www.cskaoyan.com/forum.php HTTP/1.1`

GET:请求方法

`http://www.cskaoyan.com/forum.php `  请求的url

url：http://是http协议 + www.cskaoyan.com  ip + 端口 + forum.php 服务器内部路径

 `HTTP/1.1`  http协议和版本

### 1.6.2 请求头

`Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3`

请求头部由关键字/值对组成，每行一对，关键字和值用英文冒号“:”分隔。请求头部通知服务器有关于客户端请求的信息，典型的请求头有：

•    Accept-Charset: 浏览器通过这个头告诉服务器，它支持哪种字符集

•    Accept-Encoding:浏览器能够进行解码的数据编码方式，比如gzip 

•    Accept-Language: 浏览器所希望的语言种类，当服务器能够提供一种以上的语言版本时要用到。 可以在浏览器中进行设置。

•    Host:初始URL中的主机和端口

•    Referer:包含一个URL，用户从该URL代表的页面出发访问当前请求的页面 （防盗链）

•    If-Modified-Since: Wed, 02 Feb 2011 12:04:56 GMT 服务器利用这个头与服务器的文件进行比对，如果一致，则告诉浏览器从缓存中	  直接读取文件。

•    User-Agent:浏览器类型

•    Content-Length:表示请求消息正文的长度

•    Connection:表示是否需要持久连接。如果服务器看到这里的值为“Keep -Alive”，HTTP 1.1默认进行持久连接 

[^持久连接]: tcp协议,三次握手, 四次挥手. 短时间内不断开连接, http1.0和http1.1最重要的一个区别

•    Cookie:这是最重要的请求头信息之一 

•    Date：Date: Mon, 22 Aug 2011 01:55:39 GMT请求时间GMT

Cookie:我们设置的一个信息, 用户的各种验证, 并且只有服务器能识别.我们用户在登录(数据给到服务器) -- 服务器判断我们能不能登录 -

如果可以, 那么它会给我们一个认证信息 , 就是cookie, 返回给我们 --浏览器就会存储这个cookie , 当每一次再访问这个ip+端口的时候, 携

带上这个cookie

### 1.6.3 请求数据

一般post请求的参数放到正文里

## 1.7  响应报文

```html
HTTP/1.1 200 OK
Date: Sat, 31 Dec 2005 23:59:59 GMT
Content-Type: text/html;charset=ISO-8859-1
Content-Length: 122

＜html＞
＜head＞
＜title＞Wrox Homepage＜/title＞
＜/head＞
＜body＞
＜!-- body goes here --＞
＜/body＞
＜/html＞
```

### 1.7.1 状态行

如下所示，HTTP响应的格式与请求的格式十分类似：

＜status-line＞

＜headers＞

＜blank line＞

[＜response-body＞]

 正如你所见，在响应中唯一真正的区别在于第一行中用状态信息代替了请求信息。状态行（status line）通过提供一个状态码来说明所请求的资源情况。

### 1.7.2  状态码

`HTTP/1.1 200 OK`

- 1xx：指示信息--表示请求已接收，继续处理。

- 2xx：成功--表示请求已被成功接收、理解、接受。

- 3xx：重定向--要完成请求必须进行更进一步的操作。

- 4xx：客户端错误--请求有语法错误或请求无法实现。

- 5xx：服务器端错误--服务器未能实现合法的请求。

  常见的状态代码

- 200 OK：客户端请求成功。

- 400 Bad Request：客户端请求有语法错误，不能被服务器所理解。

- 401 Unauthorized：请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用。

- 403 Forbidden：服务器收到请求，但是拒绝提供服务。

- 404 Not Found：请求资源不存在，举个例子：输入了错误的URL。

- 500 Internal Server Error：服务器发生不可预期的错误。

- 503 Server Unavailable：服务器当前不能处理客户端的请求，一段时间后可能恢复正常，举个例子：HTTP/1.1 200 OK（CRLF）

### 1.7.3 响应正文

非常常用

## 1.8 https和http区别

对称加密：以某种方式加密, 就以某种方式解密 (可以通过加密方式推断出, 解密方式) 

例如：1111 ---+2--- 1111

非对称加密：加密是一种方式, 解密是一种方式. 不可逆（公钥加密  私钥解密）

例如：111 -> 22

1.证书 : 一些权威结构颁发的证书

2.加密: 混合加密

[^混合加密]: 用非对称加密 + 对称加密

怎么进行混合加密?

1:  客户端, 发起一个请求, 请求服务器的公钥(非对称加密的加密方式)

2:   服务器把服务器的公钥返回给客户端

3:   客户端, 用服务器的公钥加密自己的加密方式(浏览器对称加密)

4:   把自己经过加密(服务器公钥加密的) 自己加密的方式给服务器

5:   服务器开始解密,用私钥, 获得了浏览器的加密方式

6:   以后, 客户端就和服务器通过 浏览器的对称加密方式通信

<img src="C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200617214749398.png" alt="image-20200617214749398" style="zoom:150%;" />

3.完整性保障:   数据被改变, 可以立即被发现, 证书认证