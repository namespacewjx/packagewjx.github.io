# HTTP学习笔记

基于[《图解HTTP》](https://www.amazon.cn/%E5%9B%BE%E4%B9%A6/dp/B00JTQK1L4/)制作成。


# 第一章 了解Web及网络基础

HTTP/1.1修订版文档：[RFC2616](http://www.ietf.org/rfc/rfc2616.txt)

通常使用的网络是在TCP/IP协议族的基础上运作的，而HTTP属于它内部的一个子集。

TCP/IP里最重要的一点就是分层。分为以下四层
- 应用层：决定了向用户提供应用服务时通信的活动
- 传输层：对上层应用层，提供处于网络连接中的两台计算机之间的数据传输
- 网络层：处理网络上流动的数据包。数据包是网络传输的最小数据单位。该层决定了通过什么样的路径到达对方计算机，并将数据包传送给对方。
- 链路层：处理网络连接的硬件部分。

与HTTP密不可分的三个协议：IP、TCP和DNS。这里不再列出。

## URI

URI是Uniform Resource Identifier，统一资源标识符。Uniform规定了统一的格式，可方便处理多种不同类型的资源。资源是指可标识的任何东西。Identifier表示可标识的对象。URI用字符串标识某一互联网资源，而URL表示资源的地点。因此URL是URI的子集。

URI的格式

`http://user:pass@www.example.cn:80/dir/index.htm?uid=1#ch1`

字段及含义
- `http://`：协议方案名。
- `user:pass`：登录信息（认证），获取服务器资源时必要的登录信息（身份认证），可选。
- `www.example.cn`：服务器地址，可以使用域名，IPv4或IPv6地址。
- `:80`：端口号。HTTP协议使用80端口。
- `/dir/index.htm`：带层次的文件路径，访问特定位置的资源，与UNIX的文件目录结构类似。
- `?uid=1`：查询字符串，传入查询的参数
- `#ch1`：片段标识符，标记处获取资源中的子资源

# 第二章 简单的HTTP协议

## 请求与响应报文

HTTP用于客户端与服务器之间的通信。请求访问资源的一端为客户端，提供资源的为服务器端。

HTTP请求示例

    GET /index.html HTTP/1.1
    Host: baidu.com

可以尝试使用telnet来给服务器发送HTTP请求，查看返回是不是网页。

请求报文是由请求方法、请求URI、协议版本、可选的请求首部字段和内容实体构成的。

方法名和协议名必须是大写。HTTP/1.1规定必须有Host字段。URI处使用`*`代表对服务器做出请求。

HTTP响应报文示例

    HTTP/1.1 200 OK
    Date: Wed, 19 Apr 2017 06:53:36 GMT
    Content-Length: 362
    Content-Type: text/html
      
    <html>
    ...

响应报文是由协议版本、状态码（表示请求成功或失败的数字代码）、用以解释状态码的原因短语、可选的响应首部字段以及实体主体构成。

## HTTP是无状态协议

HTTP协议自身不对请求和响应之间的通信状态进行保存，对发送过的请求或相应不做持久化处理。

优点：能够快速处理大量事务，确保协议的可伸缩性。
缺点：如果需要记录登录信息和身份认证信息，需要更多的额外工作

引入Cookie技术之后，就能够管理状态了。

## HTTP使用URI定位互联网上的资源

## HTTP方法

|方法|作用|协议版本|
|**GET**|请求访问被URI识别的资源|1.0、1.1|
|**POST**|传输实体的主体，向服务器传输数据|1.0、1.1|
|PUT|传输文件，但是因为不带验证机制，存在安全问题，很少使用|1.0、1.1|
|**HEAD**|和GET一样，但是不返回报文主体部分|1.0、1.1|
|DELETE|删除文件，但跟PUT问题一样，很少使用|1.0、1.1|
|OPTIONS|请求URI指定资源支持的HTTP方法|1.1|
|TRACE|类似tracert，让服务器将请求通信环回给客户端，Max-Forwards字段控制经过的节点数。但是容易引发跨站追踪攻击，很少使用|1.1|
|CONNECT|要求用隧道协议连接代理，与代理服务器通信使用，使用SSL和TSL协议传输数据|1.1|
|LINK|建立与资源之间的联系|1.0|
|UNLINE|断开连接关系|1.0|

