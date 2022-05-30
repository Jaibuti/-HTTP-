@[toc]
# 索引

[【图解HTTP】|【01】简单了解HTTP协议](https://blog.csdn.net/weixin_45926547/article/details/125011213?spm=1001.2014.3001.5501)
[【图解HTTP】|【02】HTTP报文内的HTTP信息](https://blog.csdn.net/weixin_45926547/article/details/125011894?spm=1001.2014.3001.5501)
[【图解HTTP】|【03】返回结果的HTTP状态码](https://blog.csdn.net/weixin_45926547/article/details/125012914?spm=1001.2014.3001.5501)
[【图解HTTP】|【04】与HTTP协作的Web服务器（代理、网关、隧道）](https://blog.csdn.net/weixin_45926547/article/details/125013269?spm=1001.2014.3001.5501)
[【图解HTTP】|【05】HTTP首部详解](https://blog.csdn.net/weixin_45926547/article/details/125015117?spm=1001.2014.3001.5501)
[【图解HTTP】|【06】确保web安全的HTTPS](https://blog.csdn.net/weixin_45926547/article/details/125019486?spm=1001.2014.3001.5501)
[【图解HTTP】|【07】确认访问用户身份的认证](https://blog.csdn.net/weixin_45926547/article/details/125020525?spm=1001.2014.3001.5501)
[【图解HTTP】|【08】基于HTTP的功能追加协议](https://blog.csdn.net/weixin_45926547/article/details/125022468?spm=1001.2014.3001.5501)
[【图解HTTP】|【09】Web的攻击技术](https://blog.csdn.net/weixin_45926547/article/details/125024732?spm=1001.2014.3001.5501)
## 1、用单台虚拟机实现多个域名
```
HTTP/1.1允许一台HTTP服务器搭建多个Web站点，由于利用虚拟主机的功能；

域名通过DNS服务器映射到IP地址访问目标，但一台服务器托管多个域名，该如何区分请求哪个域名呢？
- 在相同的IP地址下，发送请求时，必须在Host首部内完整指定主机名或域名的URI；
```

## 2、通信数据转发程序：代理、网关、隧道
```
【代理】：具有转发功能的应用程序，于服务器和客户端的中间人；
	接收由客户端发送的请求并转发给服务器，同时也接收服务器返回的响应并转发给客户端；
【网关】：转发其他服务器通信数据的服务器，接收从客户端发送来的请求时，将自己当作源服务器来处理请求；
【隧道】：在相隔甚远的clent和server之间进行中转，并保持双方通信连接的应用程序；
```
**代理**
![在这里插入图片描述](https://img-blog.csdnimg.cn/98b47ab9c51e4755a700323c0818d3a5.png)
```
不会改变请求的URI，直接发送给前方持有资源的目标服务器；
```
[参考：缓存代理服务器](https://blog.csdn.net/weixin_45926547/article/details/124992337)

**网关**
![在这里插入图片描述](https://img-blog.csdnimg.cn/926933f1f4c74371b6e6bf620b52d9e2.png)

```
能使通信线路上的服务器提供非HTTP协议服务，利用网关的安全性，可以在clent和网关之间的通信线路上加密以确保连接安全；
```

**隧道**
![在这里插入图片描述](https://img-blog.csdnimg.cn/b7cd4bd7bde94138be79a387658a9f5d.png)

```
可按要求建立一条与其他服务器的通信线路，届时使用SSL等加密进行通信；
是为了确保客户端能与服务器进行安全通信；
	隧道本身不会去解析HTTP请求，请求保持原样中转给之后的服务器；
	隧道会在通信双方断开连接时结束；
```