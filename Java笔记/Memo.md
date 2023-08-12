Java数据类型

基本数据类型 ：数值型（整数，浮点），字符，布尔

引用数据类型：类，接口，数组



#### Java项目和web项目的区别

java（jar）项目是有main()方法来开始的，直接依赖JVM就能编译执行。Java项目不需要服务器。

Web（war）项目中的java文件是tomcat服务器来触发的，脱离了web服务器就无法启动。Web项目需要服务器具。Web项目部署到服务器早上，任何用户都可以通过浏览器来访问。将 本地资源共享给外部访问。

#### 使用服务器

Tomcat服务器对Servlet，Jsp，JNDI，JavaMail有很好的支持，并且这个Web容易是开源免费的。（Apache开源免费）



#### Apache服务器和tomcat服务器区别

1.概述

Apache与Tomcat都是Apache开源组织开发的用于处理HTTP服务的项目，两者都是免费的，都可以做为独立的Web服务器运行。Apache是Web服务器而Tomcat是Java应用服务器。

2.具体区分

Apache服务器 只处理 静态HTML；tomcat服务器 静态HTML 动态 JSP Servlet 都能处理。

一般是把 Apache服务器与tomcat服务器 搭配在一起用。 Apache服务器负责处理所有 静态的页面/图片等信息。Tomcat只处理动态的部分。

（1）Apache：是C语言实现的，专门用来提供HTTP服务。

特性：简单、速度快、性能稳定、可配置（代理）

1、主要用于解析静态文本，并发性能高，侧重于HTTP服务；

2、支持静态页（HTML），不支持动态请求如：CGI、Servlet/JSP、PHP、ASP等；

3、具有很强的可扩展性，可以通过插件支持PHP，还可以单向Apache连接Tomcat实现连通；

4、Apache是世界使用排名第一的Web服务器。

（2）Tomcat：是Java开发的一个符合JavaEE的Servlet规范的JSP服务器（Servlet容器），是 Apache 的扩展。

特性：免费的Java应用服务器

1、主要用于解析JSP/Servlet，侧重于Servlet引擎；

2、支持静态页，但效率没有Apache高；支持Servlet、JSP请求；

3、Tomcat本身也内置了一个HTTP服务器用于支持静态内容，可以通过Tomcat的配置管理工具实现与Apache整合。

3.Apache +和Tomcat组合

两者整合后优点：如果请求是静态网页则由Apache处理，并将结果返回；如果是动态请求，Apache会将解析工作转发给Tomcat处理，Tomcat处理后将结果通过Apache返回。这样可以达到分工合作，实现负载远衡，提高系统的性能。

4.总结

apache是web服务器，tomcat是应用（java）服务器，它只是一个servlet容器，可以认为是apache的扩展，但是可以独立于apache运行。

换句话说，apache是一辆卡车，上面可以装一些东西如html等。但是不能装水，要装水必须要有容器（桶），而这个桶也可以不放在卡车上。



## SSM ：spring srpingmvc mybatis

**spring：**ioc/di 创建程序中的实例对象

**springmvc：**程序与前端交互。 浅显理解：目前项目是否可以获取前端传过来的数据。目前可以响应内容到前端。

​					   是对于servlet的封装，依赖于tomcat服务器

**mybatis：**程序与数据库交互

**补充：**

Servlet

Servlet是JavaEE规范的一种，主要是为了扩展Java作为Web服务的功能。为了方便第三方准守这种规范，Sun公司（现在Oracle公司）提供了一系列相关的接口，即Servlet API。

Servlet应用 

Servlet应用 直接或间接实现了Servlet接口并且需要运行在Servlet容器中的Java程序，主要用来生成动态的Web页面。Servlet应用不能独立于运行，必须被部署到Servlet容器。

Servlet容器

Servlet容器（Servlet引擎）是Web服务器或应用程序服务器的一部分，用于在发送的请求和响应之上提供网络服务，解码基于MIME的请求，格式化基于MIME的响应，即Servlet容器用来接收客户端请求，处理协议、请求内容等，初始化Servlet实例（只需要第一次初始化）并调用Servlet应用的对应方法，然后Servlet应用返回处理结果，经Servlet容器再返回到用户客户端。

Tomcat容器

Tomcat容器，又叫应用服务器，也有人称之为Servlet容器。其实，本质上，Tomcat容器具有Servlet容器的功能，是Servlet容器的一种开源实现，但是它又不仅仅只是Servlet容器。

## 请求响应介绍-HTTP响应格式

![Screenshot 2023-07-26 at 22.17.56](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-26 at 22.17.56.png)
