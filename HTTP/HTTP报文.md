## 报文组成
- 起始行
- 首部header
- 主体body

## 报文语法
### 请求报文
```
<method> <request-URL> <version>
<headers>

<entity-body>
```
### 响应报文
```
<version> <status> <reason-phrase>
<headers>

<entity-body>
```
## 状态码
- 200
- 3xx
301: moved permanently，请求的URL已被移除时使用。响应的Location首部中应该包含资源现在所处的URL  
302：Found，客户端应使用Location首部给出的URL来临时定位资源。将来的请求仍使用老的URL  
303：See Other，告知客户端用另一个URL来获取资源。新的URL位于响应报文的Location首部，主要目的是允许POST请求的响应将客户端定向到某个资源上去  
304：Not Modified，资源未被修改，带有这个状态码的响应不应该包含主体部分。  
- 4xx
401：Unautohorized  
403：Forbidden，请求被服务器拒绝  
404：Not Found  
- 5xx
500：Internal Server Error  
501：Not Implemented  
503：Service Unavailable  

## 协议
[internet 协议入门](https://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651551486&idx=1&sn=546b3094ddfa8c204e2cf58f12c7b0ca&chksm=8025a13fb7522829a938344e44261651a699adf1be44583cec9c81f0c516935a9679cf4f4d2f&mpshare=1&scene=1&srcid=0905gB2OjLx7qQwTCiwDE8jg&key=9379f452b81f769e35fbd9137416a63fc8d5f84e1740df35dd6ff93670d1a51761b5b62d3815dd3040f341bbaf26680c25b52106866dbe116c5063b08c5a29df4db33753b0f7d23e730d976799b9f69c&ascene=0&uin=MTM4NTE3MDA2MQ%3D%3D&devicetype=iMac+MacBookAir7%2C1+OSX+OSX+10.10.5+build(14F2009)&version=12020810&nettype=WIFI&fontScale=100&pass_ticket=IYHSZCyZuwxTulNSdDtJtMWX5RK975YF%2Fq47vei1I5HX9GI1oN%2Bsk0ujRcZWGkw9)  

### OSI七层模型  
应用层=》表示层=》会话层=》传输层=》网络层=》数据链路层=》物理层  

### TCP/IP模型
应用层=》传输层=》网络层=》数据链路层=》物理层  

### 数据链路层  
- 以太网协议  
一组电信号构成一个数据包，叫做”帧”（Frame）。  
每一帧分成两个部分：标头（Head）和数据（Data）。  
标头包含数据包的一些说明项，比如发送者、接受者、数据类型等等；数据则是数据包的具体内容。  
标头的长度，固定为18字节。数据的长度，最短为46字节，最长为1500字节。因此，整个帧最短为64字节，最长为1518字节。
如果数据很长，就必须分割成多个帧进行发送。  
- MAC地址  
网卡是以太网规定的来标明发送者和接受者信息的工具。  
网卡的地址，就是数据包的发送地址和接收地址，这叫做MAC地址。  
- 广播  
以太网采用了一种广撒网的方式，发送者发送的数据包会发送给本网络内所有的计算机，然后由接收到数据包的计算机来判断自己是不是接收方。如果是，就接受这个包，不是则丢弃。这种发送数据的方式就是广播。  

### 网络层
广播的方式只能在子网络内进行，不同子网络之间广播方式是行不通的。  
网络层所做的就是本机通过IP地址寻找到目标主机的MAC地址
- IP协议
规定网络地址的协议，叫做IP协议。它所定义的地址，就被称为IP地址。  
IP协议第四版，简称IPv4。这个版本规定，网络地址由32个二进制位组成，IPV6则是64个二进制组成。  
IP地址的表示：0.0.0.0 —— 255.255.255.255  
- ARP协议  
能通过IP地址得知MAC地址的机制  

### 传输层  
端口是每一个使用网卡的程序的编号。每个数据包都发到主机的特定端口，所以不同的程序就能取到自己所需要的数据。  

不同的程序在计算机中所占用的端口是不同的，HTTP所占用的端口一般是80，HTTPS所占用的端口一般是443。

端口是0到65535之间的一个整数，正好16个二进制位。0到1023的端口被系统占用，用户只能选用大于1023的端口。不管是浏览网页还是在线聊天，应用程序会随机选用一个端口，然后与服务器的相应端口联系。  

传输层实现的是端对端的服务，网络层实现的仅仅是主机到主机之间的服务。只要确定主机和端口，我们就能实现程序之间的交流。因此，Unix系统就把主机+端口，叫做”套接字”（socket）。
- UDP协议   
- TCP协议  
有确认机制的UDP协议，每发出一个数据包都要求确认。如果有一个数据包遗失，就收不到确认，发出方就知道有必要重发这个数据包了。

![TCP&UDP](http://www.cleey.com/Public/image/blog/20150515171004_5555b7ec09f34.png)  
![TCP7udp](http://img.blog.csdn.net/20151018103115179)

TCP协议能够确保数据不会遗失。它的缺点是过程复杂、实现困难、消耗较多的资源。
TCP数据包和UDP数据包一样，都是内嵌在IP数据包的”数据”部分。TCP数据包没有长度限制，理论上可以无限长，但是为了保证网络的效率，通常TCP数据包的长度不会超过IP数据包的长度，以确保单个TCP数据包不必再分割。  

TCP的3次握手和4次挥手  
[通俗大白话来理解TCP协议的三次握手和四次分手](https://github.com/jawil/blog/issues/14)

### 应用层  
“应用层”的作用，就是规定应用程序的数据格式。  
- DNS协议  
DNS（Domain Name System）是可以将域名和IP地址相互映射的一个分布式数据库。  

解析过程：浏览器缓存=》本机hosts=》路由器缓存=》ISP（互联网供应商）DNS缓存=》根域名服务器
- HTTP协议  

## HTTPS & 加密
HTTPS在应用层和传输层之间加入安全层SSL  
### 数字加密  
- 对称密钥加密  
编码和解码使用相同的密钥  

发送者和接受者在互相对话之前，一定要有一个共享的保密密钥  

- 非对称密钥加密  
公开密钥加密  
编码密钥是公开的，解码密钥是保密的  

避免了对称密钥加密中成对密钥数目的扩展问题  
### HTTPS  
未加密的HTTP事务：建立到服务器端口80的TCP链接                =》在TCP上发送HTTP请求=》在TCP上发送HTTP响应=》         =》TCP链接关闭

加密的HTTPS事务：建立到服务器端口443的TCP链接=》SSL安全参数握手=>在SSL上发送加密请求=》在SSL上发送已加密响应=》SSL关闭通知=》TCP链接关闭

客户端与服务器的SSL握手：  
交换协议版本号，选择密码，对两端身份进行验证，生成临时会话密钥

![加密过程](http://mmbiz.qpic.cn/mmbiz/kn3fIZB16MoJ8WfwPrx6F6guQbjia7XlibwgWrFE7HmVfASugFhNQsDdzzT9GCqOwIIhAXafmoQ26G88TX0kv9wg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

[HTTPS为什么安全 &分析 HTTPS 连接建立全过程](http://wetest.qq.com/lab/view/110.html)

## 在浏览器地址栏输入一个URL后回车，背后会进行哪些技术步骤？
[What really happens when you navigate to a URL](http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/)
