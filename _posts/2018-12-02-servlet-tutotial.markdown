---
layout: post
title: Servlet Tutotial
date: 2018-12-02 23:36:00.000000000 +09:00
---

#### 1.Servelet是什么 

​	*sevlet是Server与Applet 的缩写，即服务端小程序。Sun公司提供的开发动态web资源的技术。*

​	*servelet本质是java类，但遵循Servlet规范，没有main方法，创建、使用、销毁都在Servlet容器，如：Tomcat, Jetty, resin, Oracle Application server, WebLogic Server, Glassfish, Websphere, JBoss等*

​	*Servlet是和HTTP协议是紧密联系的，其可以处理HTTP协议相关的所有内容。*

*java applet，java应用小程序，运行在客户端的Java程序组件，运行于特定“容器”。*

**Java Servlet**是运行在Web服务器或应用服务器上的程序。它是Web浏览器或其他http请求与服务器上数据库、应用程序的中间层。

应用：使用servlet，可以收集网页表单的用户输入，呈现数据库或其他数据源的记录，还支持动态创建网页，架构图如下所示：

![img](https://img2018.cnblogs.com/blog/764465/201812/764465-20181202213307028-1121899144.png)

**Servlet主要任务**：

- 读取客户端发送的**显式数据**，包括网页上Html表单、applet或自定义的客户端程序表单；
- 读取客户端发送的**隐式数据**，包括cookies、媒体类型和浏览器理解的压缩格式。
- 处理数据，包括访问db、执行RMI或CORBA调用、调用Web服务，或者直接计算得到响应；
- 发送**显式数据（文档）**到客户端，包括文本文件html xml、二进制文件gif、excel等；
- 发送**隐式数据**到客户端，包括文档类型、设置cookies和缓存参数等

**生命周期**

- **init()** 初始化
- **service()** 方法来处理客户端的请求
- **destroy()** 方法终止（结束）
- **Servlet**是由 JVM 的垃圾回收器**进行垃圾回收**的



如下图，请求到达服务器后被委派给Servlet容器，容器加载Servlet实例并调用service()方法，容器处理多个请求线程，每个线程执行单一servlet实例的service方法。

![img](https://img2018.cnblogs.com/blog/764465/201812/764465-20181202213443567-1916420143.png)

*Servlet创建于用户第一次调用，但也可指定在服务器启动时加载；当用户调用一个servlet，对应产生一个servlet实例，每个用户请求产生一个线程。* 

*init()：创建和加载的数据，存在于servlet整个生命周期*

*service() ：实际执行任务的方法。容器调用service()，该方法检查HTTP类型并调对应方法（doPost, doGet, doPut, doDelete）；*

*destroy()：仅在 Servlet 生命周期结束时被调用。destroy()方法可以让您的 Servlet关闭数据库连接、停止后台线程、把 Cookie列表或点击计数器写入到磁盘，并执行其他类似的清理活动。*

#### 2. Servlet Demo

javac 编译生成HelloWorld.class文件

Servlet部署：

*Servlet应用程序位于路径 <Tomcat-installation-directory>/webapps/ROOT下，且类文件放在 <Tomcat-installation-directory>/webapps/ROOT/WEB-INF/classes中，并在<Tomcat-installation-directory>/webapps/ROOT/WEB-INF/ 的 web.xml 文件中创建以下*

```xml


<web-app>

    <servlet>
        <servlet-name>HelloWorld</servlet-name>
        <servlet-class>HelloWorld</servlet-class>
    </servlet>

<servlet-mapping>
    <servlet-name>HelloWorld</servlet-name>
    <url-pattern>/HelloWorld</url-pattern>
</servlet-mapping>

</web-app>


```

Get demo代码:



```java
*import javax.servlet.*;

*import javax.servlet.http.*;

*public class HelloWorld extends HttpServlet {*

  private String message;

  @Override
  public void init() throws ServletException
  {
      message = "Hello World, Nice to meet you: " + System.currentTimeMillis();
      System.out.println("servlet init ...");
      super.init();
  }

  @Override
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      response.setContentType("text/html");

  PrintWriter out = response.getWriter();
  out.println("<h1>" + message + "</h1>");
  destroy();

  }

  *@Override*
  *public void destroy()*
  *{*
      *System.out.println("servlet destroy ...");*
      *super.destroy();*
  *}*
*}*


```

启动tomcat，在浏览器查看结果如下，分析可知 servlet 销毁后，并没有立即回收，再次请求时，并没有立即初始化。

![img](https://img2018.cnblogs.com/blog/764465/201812/764465-20181202214009405-34608800.png)

#### 3.Servlet Form Data

常常需要从浏览器给web服务器（后端程序）传递信息，浏览器使用POST和GET方法。

**GET****方法**：请求字符串限制1024 字符，信息使用QUERY_STRING 头传递，并可通过环境变量QUERY_STRING 获取；一般不传递密码等敏感信息。

`http://www.test.com/hello?key1=value1&key2=value2`

**POST****方法**： Post是更可靠的方法，Post会把信息作为单独消息，以标准形式传给后台，不会像Get那样全部拼接在请求字符串中。

**Get方法demo**

下面是一个简单的 URL，将使用 GET 方法向 HelloForm 程序传递两个值。

`http://localhost:8080/TomcatTest/HelloForm?name=leeyanseven&url= leeyanseven.github.io`

*

```java
*import java.io.IOException;*
*import java.io.PrintWriter;*

*import javax.servlet.ServletException;*
*import javax.servlet.annotation.WebServlet;*
*import javax.servlet.http.HttpServlet;*
*import javax.servlet.http.HttpServletRequest;*
*import javax.servlet.http.HttpServletResponse;*

*@WebServlet("/HelloForm")*
*public class HelloForm extends HttpServlet {*
*private static final long serialVersionUID = 1L;*
*public HelloForm() {*
*super();*
*// TODO Auto-generated constructor stub*
*}*

*protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {*
*// 设置响应内容类型*
*response.setContentType("text/html;charset=UTF-8");*

*PrintWriter out = response.getWriter();*
*String title = "使用 GET 方法读取表单数据";*
*// 处理中文*
*String name =new String(request.getParameter("name").getBytes("ISO8859-1"),"UTF-8");*
*String docType = "<!DOCTYPE html> \n";*
*out.println(docType +*
    "<html>\n" +
    "undefinedundefinedundefinedundefined<head><title>" + title + "</title></head>\n" +
    "<body bgcolor=\"#f0f0f0\">\n" +
    "<h1 align=\"center\">" + title + "</h1>\n" +
    "<ul>\n" +
    "  <li><b>站点名</b>："
    + name + "\n" +
    "  <li><b>网址</b>："
    + request.getParameter("url") + "\n" +
    "</ul>\n" +
    "</body></html>");
}

*// 处理 POST 方法请求的方法*
*public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {*
    *doGet(request, response);*
*}*
*}
```

