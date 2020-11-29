# HTTP和HTTPS

`HTTP协议`（HyperText Transfer Protocol，超文本传输协议）：是一种发布和接收 HTML页面的方法。

`HTTPS`（Hypertext Transfer Protocol over Secure Socket Layer）简单讲是HTTP的安全版，在HTTP下加入SSL层。

`SSL`（Secure Sockets Layer 安全套接层）主要用于Web的安全传输协议，在传输层对网络连接进行加密，保障在Internet上数据传输的安全。

- `HTTP`的端口号为`80`
- `HTTPS`的端口号为`443`

## 三种数据加密方式

- 对称密钥加密
- 非对称密钥加密
- 证书密钥解密（HTTPS采用）



# HTTP的请求与响应

HTTP通信由两部分组成： **客户端请求消息** 与 **服务器响应消息**

![02_http_pro](1 - http&https.assets/02_http_pro.jpg) 

## 浏览器发送HTTP请求的过程：

1. 当用户在浏览器的地址栏中输入一个URL并按回车键之后，浏览器会向HTTP服务器发送HTTP请求。HTTP请求主要分为“Get”和“Post”两种方法。
2. 当我们在浏览器输入URL http://www.baidu.com 的时候，浏览器发送一个Request请求去获取 http://www.baidu.com 的html文件，服务器把Response文件对象发送回给浏览器。
3. 浏览器分析Response中的 HTML，发现其中引用了很多其他文件，比如Images文件，CSS文件，JS文件。 浏览器会自动再次发送Request去获取图片，CSS文件，或者JS文件。
4. 当所有的文件都下载成功后，网页会根据HTML语法结构，完整的显示出来了。

## URL

URL（Uniform / Universal Resource Locator的缩写）：统一资源定位符，是用于完整地描述Internet上网页和其他资源的地址的一种标识方法

- 基本格式：`scheme://host[:port#]/path/…/[?query-string][#anchor]`
  - scheme：协议(例如：http, https, ftp)
  - host：服务器的IP地址或者域名
  - port#：服务器的端口（如果是走协议默认端口，缺省端口80）
  - path：访问资源的路径
  - query-string：参数，发送给http服务器的数据
  - anchor：锚（跳转到网页的指定锚点位置）

## 客户端HTTP请求

### 请求格式

客户端发送一个HTTP请求到服务器的请求消息，包括以下格式：

- 请求行
- 请求头
- 空行
- 请求数据

![01_request](1 - http&https.assets/01_request.png) 

一个典型的HTTP请求实例：

```xml
GET https://www.baidu.com/ HTTP/1.1
Host: www.baidu.com
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.99 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Referer: http://www.baidu.com/
Accept-Encoding: gzip, deflate, sdch, br
Accept-Language: zh-CN,zh;q=0.8,en;q=0.6
Cookie: BAIDUID=04E4001F34EA74AD4601512DD3C41A7B:FG=1; BIDUPSID=04E4001F34EA74AD4601512DD3C41A7B; PSTM=1470329258; MCITY=-343%3A340%3A; BDUSS=nF0MVFiMTVLcUh-Q2MxQ0M3STZGQUZ4N2hBa1FFRkIzUDI3QlBCZjg5cFdOd1pZQVFBQUFBJCQAAAAAAAAAAAEAAADpLvgG0KGyvLrcyfrG-AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFaq3ldWqt5XN; H_PS_PSSID=1447_18240_21105_21386_21454_21409_21554; BD_UPN=12314753; sug=3; sugstore=0; ORIGIN=0; bdime=0; H_PS_645EC=7e2ad3QHl181NSPbFbd7PRUCE1LlufzxrcFmwYin0E6b%2BW8bbTMKHZbDP0g; BDSVRTM=0
```



### 请求方法

 HTTP请求主要分为`Get`和`Post`两种方法

- GET是从服务器上获取数据，POST是向服务器传送数据
- GET请求参数显示，都显示在浏览器网址上，HTTP服务器根据该请求所包含URL中的参数来产生响应内容，即“Get”请求的参数是URL的一部分。 例如： `http://www.baidu.com/s?wd=Chinese`
- POST请求参数在请求体当中，消息长度没有限制而且以隐式的方式进行发送，通常用来向HTTP服务器提交量比较大的数据（比如请求中包含许多参数或者文件上传操作等），请求的参数包含在“Content-Type”消息头里，指明该消息体的媒体类型和编码，

**注意：避免使用Get方式提交表单，因为有可能会导致安全问题。 比如说在登陆表单中用Get方式，用户输入的用户名和密码将在地址栏中暴露无遗。**



### 常用请求头

- User-Agent 
  - 客户端浏览器名称
- Connection
  - 表示客户端与服务连接类型
    1. Client 发起一个包含 `Connection:keep-alive` 的请求，HTTP/1.1使用 `keep-alive` 为默认值。
    2. Server收到请求后：
       - 如果 Server 支持 keep-alive，回复一个包含 Connection:keep-alive 的响应，不关闭连接；
       - 如果 Server 不支持 keep-alive，回复一个包含 Connection:close 的响应，关闭连接。
    3. 如果client收到包含 `Connection:keep-alive` 的响应，向同一个连接发送下一个请求，直到一方主动关闭连接。
  - keep-alive在很多情况下能够重用连接，减少资源消耗，缩短响应时间，比如当浏览器需要多个文件时(比如一个HTML文件和相关的图形文件)，不需要每次都去请求建立连接



## 服务端HTTP响应

HTTP响应也由四部分组成：状态行，消息报头，空行，响应正文

![01_response](1 - http&https.assets/01_response.jpg) 

## 常用响应头

- Content-Type:text/html;charset=UTF-8
  - 告诉客户端，响应的资源文件的类型，还有字符编码，客户端通过utf-8对资源进行解码，然后对资源进行html解析。通常我们会看到有些网站是乱码的，往往就是服务器端没有返回正确的编码