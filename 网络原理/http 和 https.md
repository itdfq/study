## http 和 https

---

### 介绍

1. **HTTP**：超文本传输协议，默认端口是80，通常使用`http://`进行访问，HTTP 协议以明文方式发送内容，不提供任何方式的数据加密
2. **HTTPS**:超文本传输安全协议。是一种透过计算机网络进行安全通信的传输协议。HTTPS 经由 HTTP 进行通信，但利用 SSL/TLS 来加密数据包

### 不同点

1. http明文传输，数据都是未加密，http是（SSL+HTTP）传输过程是加密的安全性比较好
2. https使用需要用到CA(Certificate Authority，数字证书认证机构) 申请证书
3. http页面响应速度比https快，主要是因为http使用了TCP 三次握手，客户端和服务器只需要交换三个包，而HTTPS除了TCP的三个包，还需要加上ssl握手需要的9个包，一共是12个包
4. https使用端口为443，http用的是80
5. https其实就是构建在ssl/TLS之上的http协议，所以https比http更要消耗服务器资源

---

## TCP

## 什么是TCP

 - 传输控制协议（TCP，Transmission Control Protocol）是一种面向连接的、可靠的、基于字节流的传输层通信协议。
 - TCP旨在适应支持多网络应用的分层协议层次结构。
   连接到不同但互连的计算机通信网络的主计算机中的成对进程之间依靠TCP提供可靠的通信服务。TCP假设它可以从较低级别的协议获得简单的，可能不可靠的数据报服务。
 - 由TCP 建立的连接叫做虚连接(virtual connection)，这是因为它们是由软件实现的，底层的系统并不对连接提供硬件或软件支持，只是两台机器上的TCP 软件模块通过交换消息来实现逻辑。

## TCP报文格式介绍

 - TCP报文段首部格式

![TCP报文首部格式](https://img-blog.csdnimg.cn/20210512131411576.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNDE3Mjc2,size_16,color_FFFFFF,t_70)
 - TCP报文段控制位介绍![控制为介绍](https://img-blog.csdnimg.cn/20210512132352749.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNDE3Mjc2,size_16,color_FFFFFF,t_70)
### 三次握手

![三次握手](https://img-blog.csdnimg.cn/20210512201518430.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNDE3Mjc2,size_16,color_FFFFFF,t_70)

 - 三次握手不足之处

![SYN红泛攻击](https://img-blog.csdnimg.cn/20210512201832751.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNDE3Mjc2,size_16,color_FFFFFF,t_70)

 ## 四次挥手
![四次握手模型](https://img-blog.csdnimg.cn/2021051220274463.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNDE3Mjc2,size_16,color_FFFFFF,t_70)

![四次握手](https://img-blog.csdnimg.cn/20210512204107495.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNDE3Mjc2,size_16,color_FFFFFF,t_70)

### 为什么要等待2MSL时间

 - MSL（Maximum Segment Lifetime），TCP允许不同的实现可以设置不同的MSL值。
 - 保证客户端发送的最后一个ACK报文能够到达服务器，因为这个ACK报文可能丢失，站在服务器的角度看来，我已经发送了		FIN+ACK报文请求断开了，客户端还没有给我回应，应该是我发送的请求断开报文它没有收到，于是服务器又会重新发送一次，而客户端就能在这个2MSL时间段内收到这个重传的报文，接着给出回应报文，并且会重启2MSL计时器。
 - 防止类似与“三次握手”中提到了的“已经失效的连接请求报文段”出现在本连接中。客户端发送完最后一个确认报文后，在这个2MSL时间中，就可以使本连接持续的时间内所产生的所有报文段都从网络中消失。这样新的连接中不会出现旧连接的请求报文。
 - 建立连接的时候，服务器在LISTEN状态下，收到建立连接请求的SYN报文后，把ACK和SYN放在一个报文里发送给客户端。而关闭连接时，服务器收到对方的FIN报文时，仅仅表示对方不再发送数据了但是还能接收数据，而自己也未必全部数据都发送给对方了，所以己方可以立即关闭，也可以发送一些数据给对方后，再发送FIN报文给对方来表示同意现在关闭连接，因此，己方ACK和FIN一般都会分开发送，从而导致多了一次。
 - TCP还设有一个保活计时器，显然，客户端如果出现故障，服务器不能一直等下去，白白浪费资源。服务器每收到一次客户端的请求后都会重新复位这个计时器，时间通常是设置为2小时，若两小时还没有收到客户端的任何数据，服务器就会发送一个探测报文段，以后每隔75秒发送一次。若一连发送10个探测报文仍然没反应，服务器就认为客户端出了故障，接着就关闭连接。

---
文章图片来源于哔哩哔哩王道考研   [计算机网络原理](https://www.bilibili.com/video/BV19E411D78Q?p=64&spm_id_from=pageDriver)
