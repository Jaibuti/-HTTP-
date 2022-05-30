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
## 1、何为认证
```
服务器与客户端核对信息：	
	- 密码：只有本人才会知道的字符串信息。
	- 动态令牌：仅限本人持有的设备内显示的一次性密码；
	- 数字证书：仅限本人(终端）持有的信息；
	- 生物认证：指纹和虹膜等本人的生理信息；
	- IC卡：仅限本人持有的信息；
【HTTP使用的认证方式】：
	- BASIC认证：基本认证；
	- DIGEST认证：摘要认证；
	- SSL客户端认证；
	- FormBase认证：基于表单；
```

## 2、BASIC认证
```
从HTTP/1.0具有；
是Web服务器与通信客户端之间进行的认证方式；
采用Base64编码方式，不需要任何附加信息即可对其解码；
安全性低；
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/80c39906bd21444da2f4b56e2b43f84d.png)
## 3、DIGEST认证
```
为了你不BASIC认证的弱点；从HTTP/1.1开始有；
使用质询响应的方式，但不像BASIC直接发送明文密码；
【质询响应方式】：一开始一方会先发送认证要求给另外一方，在使用从另一方那接收到的质询码计算生成相应码；
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/aa55394453194a17a6f1a4b021304ca8.png)
## 4、SSL客户端认证
```
SSL是借由HTTP的客户端证书完成认证的方式，凭客户端证书认证，服务器即可确认访问是否自己登录的客户端；
```
**SSL客户端认证的认证步骤**

```
- 接收到需要认证资源的请求，服务器会发送 CertificateRequest 报文，要求客户端提供客户端证书；
- 用户选择将发送的客户端证书后，客户端会把客户端证书信息以Client Certificate报文方式发送给服务器；
- 服务器验证客户端证书验证通过后方可领取证书内客户端的公开密钥，然后开始HTTPS加密通信；
```

**SSL客户端认证采用双因素认证**
```
SSL客户端认证不仅依靠证书完成认证，还会基于表单认证，进行组合；
	- 第一因素：SSL客户端证书用来认证客户端；
	- 第二因素：密码用来确认这是用户本人的行为；
```

## 5、基于表单认证
```
客户端会像服务器上的Web应用程序发送登录信息，按该登录信息验证结果认证；
```
**Session管理及Cookie应用**

```
一般使用Cookie来管理Session会话；
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/18e01de7ef97482cb10b49b72420b2d7.png)