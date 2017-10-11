---
title: JDK1.7默认支持的TLS版本导致异常
tags: 异常集
categories: 异常集
---
### 异常背景
> 运行环境配置：Linux JDK1.7+ 

> 编译环境配置: Windows7 JDK1.8+

<!-- more -->

在Windows上运行访问Https地址正常，在Linux上提示如下信息：

```
org.springframework.web.client.ResourceAccessException: I/O error on GET request for "https://www.kickstarter.com/discover/advanced?category_id=16&sort=popularity&state=live&page=1":Received fatal alert: protocol_version; nested exception is javax.net.ssl.SSLException: Received fatal alert: protocol_version
	at org.springframework.web.client.RestTemplate.doExecute(RestTemplate.java:580)
	at org.springframework.web.client.RestTemplate.execute(RestTemplate.java:530)
	at org.springframework.web.client.RestTemplate.exchange(RestTemplate.java:448)
	at cn.geekview.util.CommonUtils.httpforKickstarter(CommonUtils.java:215)
	at cn.geekview.project.grap.service.impl.KsDataGrapServiceImpl.doInitProjectUrl(KsDataGrapServiceImpl.java:134)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at org.springframework.aop.support.AopUtils.invokeJoinpointUsingReflection(AopUtils.java:317)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.invokeJoinpoint(ReflectiveMethodInvocation.java:190)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:157)
	at org.springframework.transaction.interceptor.TransactionInterceptor$1.proceedWithInvocation(TransactionInterceptor.java:99)
	at org.springframework.transaction.interceptor.TransactionAspectSupport.invokeWithinTransaction(TransactionAspectSupport.java:281)
	at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:96)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at org.springframework.aop.interceptor.ExposeInvocationInterceptor.invoke(ExposeInvocationInterceptor.java:92)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at org.springframework.aop.interceptor.AsyncExecutionInterceptor$1.call(AsyncExecutionInterceptor.java:110)
	at java.util.concurrent.FutureTask.run(FutureTask.java:262)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
	at java.lang.Thread.run(Thread.java:745)
Caused by: javax.net.ssl.SSLException: Received fatal alert: protocol_version
	at sun.security.ssl.Alerts.getSSLException(Alerts.java:208)
	at sun.security.ssl.Alerts.getSSLException(Alerts.java:154)
	at sun.security.ssl.SSLSocketImpl.recvAlert(SSLSocketImpl.java:1979)
	at sun.security.ssl.SSLSocketImpl.readRecord(SSLSocketImpl.java:1086)
	at sun.security.ssl.SSLSocketImpl.performInitialHandshake(SSLSocketImpl.java:1332)
	at sun.security.ssl.SSLSocketImpl.startHandshake(SSLSocketImpl.java:1359)
	at sun.security.ssl.SSLSocketImpl.startHandshake(SSLSocketImpl.java:1343)
	at sun.net.www.protocol.https.HttpsClient.afterConnect(HttpsClient.java:563)
	at sun.net.www.protocol.https.AbstractDelegateHttpsURLConnection.connect(AbstractDelegateHttpsURLConnection.java:185)
	at sun.net.www.protocol.https.HttpsURLConnectionImpl.connect(HttpsURLConnectionImpl.java:153)
	at org.springframework.http.client.SimpleBufferingClientHttpRequest.executeInternal(SimpleBufferingClientHttpRequest.java:81)
	at org.springframework.http.client.AbstractBufferingClientHttpRequest.executeInternal(AbstractBufferingClientHttpRequest.java:48)
	at org.springframework.http.client.AbstractClientHttpRequest.execute(AbstractClientHttpRequest.java:53)
	at org.springframework.web.client.RestTemplate.doExecute(RestTemplate.java:569)
	... 22 more
```

### 分析原因

> JDK1.7+ 默认支持的TLS版本不是TLSv1.2

> [查看JDK1.7+ 支持的协议](http://docs.oracle.com/javase/7/docs/technotes/guides/security/StandardNames.html#SSLContext)


### 解决方案

> 在发送请求前设置系统属性：System.setProperty("https.protocols", "TLSv1.2");

### 相关链接
- https://stackoverflow.com/questions/9749339/does-tomcat-support-tls-v1-2