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
## 1、HTTP报文
```
【HTTP报文】：用于HTTP协议交互的信息；
	- 报文首部和报文主体，两者这中间用空行（回车符、换行符）；
【请求报文】：客户端；
【响应报文】：服务端；
```

## 2、请求报文及响应报文的结构

![在这里插入图片描述](https://img-blog.csdnimg.cn/a1a9e7e06074469285691afb5540890f.png)
```
【请求行】：包含用于请求的方法，请求URI和HTTP版本；
【状态行】：包含表明响应结果的状态码，原因短语和HTTP版本；
【首部字段】：包含表示请求和响应的各种条件和属性的各类首部；
	- 【首部】：通用首部、请求首部、响应首部、实体首部；
```

## 3、编码提升传输速率
```
传输过程中，通过编码来提升传输速率，但编码操作需要计算机完成，会消耗更多的CPU资源；
```
**报文主题和实体主题的差异**
```
【报文】：是HTTP通信的基本单位，由8为组字节流组成，通过HTTP通信传输；
【实体】：作为请求或响应的有效载荷数据被传输，由实体首部和实体主题组成；

通常，报文主体等于实体主体，只有当传输中进行编码操作时，实体主体内容发送变化，才导致它和报文主体产生差异；
```
**压缩传输的内容编码**
```
发送邮件时，附件一般会向用ztip压缩在添加符间发送；
【内容编码】：应用在实体内容上的编码格式，并保持实体信息原样压缩，由客户端接收并负责解码；
- gzip；
- compress；
- deflate(zlib)；
- identity；
```
**分割发送的分块传输编码**
```
【分块传输编码】：传输大容量数据时，将数据分割成多块，让页面逐步显示；
	其中每一块都会用十六进制来标记块的大小，实体主体的最后一块会使用0(CR+LF)标记；
```
## 4、发送多种数据的多部分对象集合
```
【MIME】：允许邮件处理文本、图片、视频等多种类型的数据；
	该机制中会使用一种多部份对象集合的方法，来容纳多份不同类型的数据；

HTTP也采纳多部份对象集合，发送的一份报文主体内可含由多类型实体；
	使用时，需要在首部字段加上Content-type字段；
	使用boundary字符串来划分多部份对象集合指明的各类实体；
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/7620e5ba61684f348ce19326bcbca09e.png)
## 5、获取部分内容的范围请求
```
此前的网络带宽小，当下载大图片或文件若遇到网络中断将会重头开始；
故提出了【范围请求】：实现该功能需要指定下载的实体范围；
	执行范围请求时，会用到首部Range来指定资源的type范围；
	针对范围请求，响应会返回状态码为206；
	若多重范围请求，响应会在首部字段Content-Type表明multipart/byteranges返回响应报文；
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/ff7c406bc7674c99abdd77edc10e5eca.png)
## 6、内容协商返回最合适的内容
```
【内容协商机制】：当浏览器的默认语言为英文或中文，访问相同的URI的Web页面时，则会显示对应的英文版或中文版的Web页面；
	指客户端和服务器就响应的资源内容进行交涉，在提供给客户端最为合适的资源；
	该机制会以以下字段为判断基准：
		- Accept；
		- Accept-Charset；
		- Accept-Encoding；
		- Accept-Language；
		- Content-Language；

【内容协商技术】：
-  【服务器驱动协商】：由服务器进行内容协商，以请求的首部字段为参考，在服务端自动处理；
- 【客户端驱动协商】：由客户端进行内容协商方式，用户从浏览器显示的可选项列表中手动选择；
- 【透明协商】：服务器驱动和客户端的结合体，由服务器端和客户端各自进行内容协商的一种方法；
```