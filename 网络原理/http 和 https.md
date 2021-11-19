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

![image-20211119100039520](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211119100039520.png)