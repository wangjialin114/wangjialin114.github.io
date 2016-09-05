---
layout:     post
title:      "配置Tomcat服务器使Http转Https"
subtitle:   "安全不该只是纸上谈兵"
date:       2016-08-24
author:     "WangJialin"
tags:
    - java
    - web
    
---

- [1.配置https](#https)
	- [1.1. 生成证书](#cert)
	- [1.2 配置tomcat文件](#tom_conf)
- [2. 强制https访问](#force_https)
	- [2.1 配置tomcat文件(web.xml)](#tomcat_web)
- [3. 几点重要的说明](#ps)
- [4. 参考链接](#ref)


https是http+ssl的可进行加密传输，身份认证的网络协议，防止数据在传输过程中被窃取。因此，https将得到越来越广泛的应用，下面是如何配置tomcat服务器让http自动转到https的步骤。

---

<a name="https"></a>

## 1. 配置https

<a name="cert"></a>

### 1.1. 生成证书

 - 进入$JAVA_HOME/bin目录
 
 - 执行命令
 
 `./keytool -genkey -alias tomcat -keyalg RSA -keystore D:\tomcat.keystore -validity 36500`
上述命令中，只需要修改“D:\tomcat.keystore” 这是你生成证书存放的位置。“-validity 36500”含义是证书有效期，36500表示100年。
![http_https.png-15kB][1]
在生成证书过程中，可以只用输入密钥库口令，其余的都缺省。最后一个是输入<tomcat>的口令，这项较为重要，会在tomcat配置文件中使用，建议输入与keystore的密码一致，设置其它密码也可以。
另外，请最好用管理员权限运行cmd，不然生成证书时有可能出现javaFileIOexception的权限问题。

<a name="tom_conf"></a>

### 1.2 配置tomcat文件（server.xml）

打开tomcat\conf\server.xml，修改如下，

```

<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
修改参数=>
<Connector port="80" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="443" />
 
<!--
<Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
              maxThreads="150" scheme="https" secure="true"
              clientAuth="false" sslProtocol="TLS"/>
 -->
去掉注释且修改参数=>
<Connector port="443" protocol="HTTP/1.1" SSLEnabled="true"
           maxThreads="150" scheme="https" secure="true"
           clientAuth="false" sslProtocol="TLS" keystoreFile="D:\tomcat.keystore"                                     keystorePass="tomcat"/>
注释：标识为淡蓝色的两个参数，分别是证书文件的位置和<tomcat>的主密码，在证书文件生成过程中做了设置
<!--
   <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
-->
修改参数=>
<Connector port="8009"  protocol="AJP/1.3" redirectPort="443" />

```

![img](/img/2016_latter_half_year/http_https.png)

如果这样修改后，尝试在浏览器输入https网址访问，在谷歌浏览器如果出现下面的这样的问题：
提示“**此网站无法提供安全连接**”。
![安全连接.png-40.3kB][2]
导致出现这个错误的原因是server.xml文件中启用了两种ssl配置机制，APR和JAVA Connector,我们在这里采用的是JAVA Connector机制。
那么，恭喜你，你还需要对server.xml做如下修改：
将下面这一行配置代码注释掉：

```
<Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
```

如下图中箭头指的那行代码：

![](http://http://wangjialin114.github.io/img/2016_latter_half_year/keystore-apr.png)

---

<a name="force_https"></a>

## 2. 强制https访问

接下来，我们还可以更进一步使每一个访问强制使用https访问，这样你只用输入url，就会自动启用https传输协议。


<a name="tomcat_web"> </a>

### 2.1 配置tomcat文件(web.xml)

tomcat\conf\web.xml中的</welcome-file-list>后面加上这样一段：

```
<login-config>
  <!-- Authorization setting for SSL -->
	<auth-method>CLIENT-CERT</auth-method>
 	<realm-name>Client Cert Users-only Area</realm-name>
</login-config>

<security-constraint>
  <!-- Authorization setting for SSL -->
	<web-resource-collection >
		<web-resource-name >SSL</web-resource-name>
		<url-pattern>/*</url-pattern>
	</web-resource-collection>
	<user-data-constraint>
		<transport-guarantee>CONFIDENTIAL</transport-guarantee>
	</user-data-constraint>
</security-constraint>
```

结果如下图所示：

![img](/img/2016_latter_half_year/web.png)

<a name="ps"></a>

## 3. 几点重要的说明

如果你将网站部署在一些网络端口资源稀缺的环境中，端口的资源控制严格。OK，那你可能会发现无法访问的情况。比如搭建的某个服务，
`外网访问： 122.201.23.5:9924`
端口映射为：`9924->80`
`跳转：122.201.23.5:80`
假如你在配置server.xml文件时，按照我上面的步骤配置的话，在浏览器中你输入:
`122.201.23.5:9924`
你可能会遇到拒绝访问的问题。

![img](/img/2016_latter_half_year/安全连接.png)

原因在于单独输入上面的请求类似于：
`http:\\122.201.23.5:9924`
而上面是一个`http request`,那么整个过程基本上就是：
`9924->80(http)->(redirectport,https)8843`
可是有时候，你可能没有8843端口的权限，因此无法访问。
因此对于只有一个端口时，那么就只能把那个端口配置成支持https的端口，不能加上重定向。但是这个时候，你必须在浏览器中输入，

`https:\\122.201.23.5:9924`

这是一个`https request`，整个过程为：

`9924->80(https)`

我前面讲的步骤都是有重定向端口的，假如不加上重定向的话最终配置就类似如下图这样：



```
<Connector port="80" protocol="HTTP/1.1" SSLEnabled="true"
           connectionTimeout="20000"
           maxThreads="150" scheme="https" secure="true"
           clientAuth="false" sslProtocol="TLS"               
           keystoreFile="D:\tomcat.keystore"                             keystorePass="tomcat"/>
```

但是，这样对用户来说太复杂了，每次在浏览器中药用户加上`https`,讲真，有点反人类！
所以综上，说到底！！兄弟，再去申请一个端口吧，反正几万个端口再申一个难度不大。

---

<a name="ref"></a>

## 4. 参考链接

 - http://blog.sina.com.cn/s/blog_618592ea01012q40.html
 - https://tomcat.apache.org/tomcat-6.0-doc/ssl-howto.html
 - http://tomcat.apache.org/tomcat-6.0-doc/config/http.html#SSL%20Support