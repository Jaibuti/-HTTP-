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
## 1、消除HTTP瓶颈的SPDY

```
SPDY：为了解决HTTP的性能瓶颈，缩短Web页面的加载时间；
```
#### 1.1 HTTP的瓶颈

```
以下几种可能成为瓶颈：
- 一条连接上只可发送一个请求；
- 请求只能从客户端开始，客户端不可以接收除响应以外的指令；
- 请求/响应首部未经压缩就发送。首部信息越多延迟越大；
- 发送冗长的首部。，次互相发送相同的首部造成的浪费较多；
- 可任意选择数据压缩格式，非强制压缩发送;；
```
**Ajax解决方法**

```
有效利用Js和Dom的操作，达到局部Web页面替换加载的异步通信手段；
	实时地从服务器获取内容，有可能会导致大量请求产生，且未解决HTTP本身问题；
```

**Comet解法方法**
```
一旦服务器有内容更新了，Comet不会让请求等待，而是直接给客户端返回响应；
	为了实现推送功能，Comet先将响应置于挂起状态，当服务器内一旦有内容更新时，再返回该响应；
	但为了实时更新，连接的持续时间变长，消耗了更多资源，也没有解决HTTP本身问题；
```

#### 1.2 SPDY的设计与功能
![在这里插入图片描述](https://img-blog.csdnimg.cn/e018f9bcf7834c90a9f6e78045b60be7.png)

```
SPDY没有完全改写HTTP协议，而是再TCP/IP的应用层与运输层之间通过新加会话层的形式运作；
使用SPDY后，HTTP协议额外获得以下功能：
- 多路复用流：
	通过单一的TCP连接，可无限制处理多个HTTP请求，所有请求的处理都在一条TCP连接上完成，以提高TCP处理效率；
- 赋予请求优先级：
	SPDY不仅可以无限制地并发处理请求，还可给请求逐个分配优先级顺序，为了在发送多个请求时，解决带宽低而导致响应变慢的问题；
- 压缩HTTP首部：
	压缩HTTP请求和响应的首部，以此通信产生的数据包数量和发送的字节数就更少了；
- 推送功能：
	支持服务器主动向客户端推送数据的功能，服务器可直接发送数据，不必等待客户端的请求；
- 服务器提示功能：
	服务器可主动提示客户端请求所需的资源；若资源已缓存，则避免发送不必要的请求；
```

#### 1.3 SPDY消除Web瓶颈了吗

```
SPDY只是将单个域名的通信多路复用，故当一个web网站上使用多个域名下的资源，改善效果将会受到限制；
```

## 2、使用浏览器进行全双工通信的WebSocket
```
WebSocket是为了解决HTTP瓶颈问题而实现的一套新协议及API；
```

#### 2.1 WebSocket的设计与功能
```
WebSocket是Web浏览器与Web服务器之间全双工通信标准；
主要为了解决Ajax和Comet里XMLHttpRequest附带的缺陷所引起的问题；
```

#### 2.2 WebSocket协议
```
一旦Web服务器和clilent建立起用WebSocket协议的通信连接，后所以通信都依靠该协议；
	通信过程中可互相发送Json、XML、图片等任意格式的数据；
	任意一方都可直接向对方发送报文；

【推送功能】：支持由服务器向客户端推送数据的推送功能，故服务器可直接发送数据，不必等待客户端的请求；
【减少通信量】：只要建立WebSocket，就希望一直保持连接，较少每次连接的总开销；且该协议的首部信息小，通信量也减小；
```
**握手请求**
```
为了实现WebSocket通信，需要用到HTTP的Upgrade首部字段，告知服务器通信协议发送改变，已达握手；
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/c59185e2a93442dc98069878cd781c28.png)
**握手响应**
![在这里插入图片描述](https://img-blog.csdnimg.cn/4ce432ba61194f82805d3ebabc02c065.png)
```
对于101的响应；
成功建立连接后，通信时不再使用HTTP的数据帧，而采用WebSocket独立的数据帧；
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/91ead8964a4f4425bf5e661eb3d44d65.png)


## 4、HTTP/2.0

```
【特点】：
- SPDY；
- HTTP Speed + Mobility：用于改善并提高移动端通信时的通信速度和性能的标准；
- Network-Friendly HTTP Upgrade：移动端通信时改善HTTP性能的标准；
```

## 5、Web服务器管理文件的WebDAV
```
是一个可对Web服务器上的内容直接进行文件复制、编辑等操作的分布式文件相同；
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/fc7cd76178d04ab38f07b0fb5be940c2.png)
#### 5.1 扩展HTTP/1.1的WebDAV
**WebDAV新增概念**：
![在这里插入图片描述](https://img-blog.csdnimg.cn/7c02c7cd1b214c348ff5c7e92215f316.png)

```
集合：一种统一管理多个资源的概念，以集合为单位可进行各种操作；
资源：把文件或集合称为资源；
属性：定义资源的属性。定义以“名称=值”的格式执行；
锁：把文件设置成无法编辑状态。多人同时编辑时，可防止在同一时间进行内容写入；
```
#### 5.2 WebDAV内新增的方法及状态码
**为了实现远程文件管理**
```
- PROPFIND：获取属性；
- PROPPATCH：修改属性；
- MKCOL：创建集合；
- COPY：复制资源及属性；
- MOVE：移动资源；
- LOCK：资源加锁；
- UNLOCK:资源解锁；
```
**为了配合扩展的方法**
```
102：可正常处理请求，但目前是处理中状态；
207：存在多种状态；
422：格式正确，内容有误；
423：资源已被加锁；
424 ：处理与某请求关联的请求失败，因此不再维持依赖关系；
507：保存空间不足；
```
**WebDAV请求实例，使用PROPFIND方法**
![在这里插入图片描述](https://img-blog.csdnimg.cn/33e68c48115f48f8a505d2c1f21575bf.png)
**WebDAV响应实例**
![在这里插入图片描述](https://img-blog.csdnimg.cn/e340f4ef1f6e4d84bc3a1acd0d944108.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c99878c230994815b4e96bf7e2c024e5.png)