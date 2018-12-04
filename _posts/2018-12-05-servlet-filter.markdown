---
layout: post
title: Servlet Tutotial(Filter/Exception/Cookie)
date: 2018-12-05 00:39:00.000000000 +09:00
---

#### 过滤器

Servlet 过滤器可以**动态地拦截请求和响应**，以变换或使用包含在请求或响应中的信息。

过滤器首先是java类（实现javax.servlet.Filter 接口），其次是用于servlet编程。

**过滤器是"链"在容器的处理过程中的。**这就意味着它们会在servlet处理之前**访问一个进入的请求**，并且在外发响应信息返回到客户前**访问这些响应信息**。这种访问使得过滤器可以检查并修改请求和响应的内容。

根据规范**常用的过滤器**：身份验证过滤器、数据压缩过滤器、加密过滤器、触发资源访问事件过滤器、图像转换过滤器、日志记录和审核过滤器、标记化过滤器、XSL/T 过滤器。

**过滤器方法**

**doFilter()**  方法完成实际的过滤操作，当客户端请求方法与过滤器设置匹配的URL时，Servlet容器将先调用过滤器的doFilter方法

**init()** web应用程序启动时，web服务器将创建Filter 的实例对象，并调用其init方法，读取web.xml配置，完成对象的初始化功能，从而为后续的用户请求作好拦截的准备工作

**destroy()**  Servlet容器在销毁过滤器实例前调用该方法，释放资源

 

```
<filter>指定一个过滤器。

<filter-name>用于为过滤器指定一个名字，该元素的内容不能为空。

<filter-class>元素用于指定过滤器的完整的限定类名。

<init-param>元素用于为过滤器指定初始化参数，它的子元素<param-name>指定参数的名字，<param-value>指定参数的值。

在过滤器中，可以使用FilterConfig接口对象来访问初始化参数。

<filter-mapping>元素用于设置一个 Filter 所负责拦截的资源。一个Filter拦截的资源可通过两种方式来指定：Servlet 名称和资源访问的请求路径

<filter-name>子元素用于设置filter的注册名称。该值必须是在<filter>元素中声明过的过滤器的名字

<url-pattern>设置 filter 所拦截的请求路径(过滤器关联的URL样式)

<servlet-name>指定过滤器所拦截的Servlet名称。

<dispatcher>指定过滤器所拦截的资源被 Servlet 容器调用的方式，可以是REQUEST,INCLUDE,FORWARD

和ERROR之一，默认REQUEST。用户可以设置多个<dispatcher>子元素用来指定 Filter 对资源的多种调用方式进行拦截。
```

### servlet异常处理

当抛出一个异常时，Web容器在web.xml（使用 exception-type 元素）匹配异常类型。

```
<!-- exception-type 相关的错误页面 -->

<error-page>

    <exception-type>

          javax.servlet.ServletException

    </exception-type >

    <location>/ErrorHandler</location>

</error-page>

 

<error-page>

    <exception-type>java.io.IOException</exception-type >

    <location>/ErrorHandler</location>

</error-page>
```

### Cookie

Cookie 是存储在客户端计算机上的文本文件，并保留了各种跟踪信息。

```
HTTP/1.1 200 OK

Date: Fri, 04 Feb 2000 21:03:38 GMT

Server: Apache/1.3.9 (UNIX) PHP/4.0b3

Set-Cookie: name=xyz; expires=Friday, 04-Feb-07 22:03:38 GMT; 

                 path=/; domain=runoob.com

Connection: close

Content-Type: text/html
```

### Session 跟踪

Web客户端与服务器之间的请求连接。HTTP是“无状态”协议，客户端打开到web服务器的连接，服务器不保留之前的请求的记录。

维持web 客户端与服务器间session会话方法：cookies、form字段session Id、URL重写

### Servlet package 

Servlet API 规范，给定一个顶级目录名 myapp，目录结构如下所示：

```
/myapp
    /images
    /WEB-INF
        /classes
        /lib
```

WEB-INF 子目录中包含应用程序的部署描述符，名为 web.xml。所有的 HTML 文件都位于顶级目录 myapp 下。

WEB-INF/classes 目录包含了所有的 Servlet 类和其他类文件，类文件所在的目录结构与他们的包名称匹配。例如，如果您有一个完全合格的类名称 **com.myorg.MyServlet**，那么这个 Servlet 类必须位于以下目录中

```
/myapp/WEB-INF/classes/com/myorg/MyServlet.class
```

### Servlet 国际化

Servlet 可以根据请求者的区域设置拾取相应版本的网站，并根据当地的语言、文化和需求提供相应的网站版本

**国际化（i18n****）**：一个网站提供了不同版本的翻译成访问者的语言或国籍的内容。

**本地化（l10n****）**：给网站添加资源，以使其适应特定的地理或文化区域，例如网站翻译成印地文（Hindi）。

**区域设置（locale****）**：这是一个特殊的文化或地理区域。它通常指语言符号后跟一个下划线和一个国家符号。例如 "en_US" 表示针对 US 的英语区域设置。