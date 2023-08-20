# 1.Http（应用层协议）

## 1-1.Http特点

- 支持客户/服务器模式
- 简单快捷：客户向服务器请求服务时，只需传送请求方法和路径。
- 灵活：HTTP允许传输任意类型的数据对象。传输的类型由Content-Type加以标记
- 无连接
- 无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

## 1-2.HTTP-URL，请求协议与响应协议

- ```
  http://host[:prot]/[abc_path]
  ```

- HTTP请求 由三部分组成，分别是：请求行，请求头，请求正文

  - 请求行：请求方式+请求地址+请求协议版本     

  - 请求头：键值对

  - 请求正文：get没有请求正文

    

  - 格式

    ```html
    请求行
    请求头1
    请求头2
     ...
    请求空行
    请求体
    ```

- HTTP响应 由三部分组成，分别是：响应行，响应头，响应正文

  - 响应行：响应协议版本+状态码   

  - 响应头：键值对

  - 响应正文：Response中的内容

    

  - 格式

    ```html
    响应行
    响应头1
    响应头2
     ...
    响应空行
    响应正文
    ```

    

# 2.Tomcat

Tomcat简单的说就是一个运行Java的网络服务器，底层是Socket的一个程序，它也是jsp和Servlet的一个服务器。



# 3.Servlet的实现

Servlet是Server与Applet的缩写，是服务器小程序的意思。使用Java语言编写的服务器端程序，可以像生成动态的WEB页，Servlet主要运行在服务器端，并由服务器调用执行，是一种按照Servlet标准来开发的类。是SUN公司提供的一门开发动态WEB资源的技术。（言外之意：要实现web开发，需要实现Servlet标准）

Servlet本质上也是Java类，但是要遵循Servlet规范进行编写，没有main()方法，它的创建，使用，销毁都由Servlet容器进行管理（如Tomcat）。（言外之意：写自己的类，不用写main方法，别人自动调用）

Servlet是和HTTP协议时紧密联系的，其可以处理HTTP协议相关的所有内容。这也是Servlet应用广泛的原因之一。

提供了Servlet功能的服务器，叫做Servlet容器，其常见的容器有很多，入Tomcat，jetty，WebLogic Server，

WebSphere，JBoss等等。

```html
实现Servlet
1.创建普通的java类
2.实现Servlet的规范，继承HttpServelt类
3.重写service(或doGet/doPost)方法，用来处理请求
4.设置注解，指定访问的路径
```

### 3-1.创建web项目

## 3-2.实现Servlet规范

实现Servlet规范，即继承HttpServlet类，并到如响应的包，该类中已经完成了通信的规则，我们只需要进行业务的实现即可

```java
package com.xxxx.servlet;
import javax.servlet.http.HttpServlet;
public class Servlet01 extends HttpServlet{
	...
}
```

## 3-3.重写service方法

满足Servlet规范只是让我们的类能够满足接收请求的要求，接收到请求后需要对请求进行分析，以及进行业务逻辑处理，计算出结果，则需要添加代码，在规范中有个一叫做service的方法，专门用来做请求处理的操作，业务代码则可以写在该方法中。

```java
package com.xxxx.servlet;
import javax.servlet.http.HttpServlet;
public class Servlet01 extends HttpServlet{

}
```

## 3-4.设置注解

在完成好了一切代码的编写后，还需要向服务器说明，特定请求对应特定的资源

开发servlet项目，使用@WebServlet将一个继承于javax.servlet.http.HttpServlet的类定义为Servlet组件。

在Servlet3.0中，可以使用@WebServlet注解将一个继承于javax.servlet.http.HttpServlet的类标注为可以处理用户请求的Servlet。

```java
@WebServlet("/hello-servlet")
public class HelloServlet extends HttpServlet {

    public void service(HttpServletRequest request, HttpServletResponse response) throws IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("<html><body>");
        out.println("<h1>" + "Hello Servlet"+  "</h1>");
        out.println("</body></html>");
    }
```

# 4.服务器设置与WebServlet注解

服务器设置：

```
Run/Debug Configurations
	Server
	Deployment
```

webServlet注解

```
//方式一
@WebServlet("/hello-servlet")
//方式二
@WebServlet(name = "hello-servlet",value="/hello-servlet")
//方式三
@WebServlet(name = "hello-servlet",value={"/hello-servlet","/hello"})
//方式四
urlPatterns与value等价，可以互换
```

# 5.Servlet工作流程

Servlet 是 Java 编写的用于处理 Web 请求和生成动态 Web 内容的组件。下面是 Servlet 的典型工作流程：

1. **客户端发送请求：** 客户端（通常是浏览器）发送 HTTP 请求到 Web 服务器，请求一个特定的 URL（Uniform Resource Locator）。
2. **Web 服务器接收请求：** Web 服务器接收到请求后，根据请求的 URL 找到对应的 Servlet 组件。
3. **Servlet 容器加载 Servlet：** Web 服务器中的 Servlet 容器（例如 Tomcat）负责加载和管理 Servlet。如果该 Servlet 还没有被加载，则容器加载 Servlet 类并初始化一个 Servlet 实例。
4. **Servlet 初始化：** 在加载时，Servlet 容器会调用 Servlet 的 `init` 方法，完成一些初始化设置，例如读取配置、连接数据库等。这个步骤只会在 Servlet 第一次被加载时执行。
5. **Servlet 处理请求：** 一旦初始化完成，Servlet 容器会根据请求类型（GET、POST 等）调用 Servlet 的相应方法（如 `doGet` 或 `doPost`）。
6. **处理业务逻辑：** 在 `doGet` 或 `doPost` 方法中，你可以编写处理业务逻辑的代码。这可能涉及到从请求中读取参数、与数据库交互、生成动态内容等。
7. **生成响应：** 在处理业务逻辑后，Servlet 可以生成响应内容。这通常是 HTML、JSON、XML 等格式的内容，可以是静态的，也可以是根据请求生成的动态内容。
8. **设置响应头：** Servlet 可以设置响应的头信息，如设置响应类型、字符编码、缓存控制等。
9. **发送响应：** Servlet 将生成的响应内容发送回客户端，响应会经过 Web 服务器传输到客户端。
10. **Servlet 完成处理：** 一旦响应发送完成，Servlet 容器会调用 Servlet 的 `destroy` 方法，释放资源并完成 Servlet 的生命周期。
11. **响应到达客户端：** 响应内容到达客户端，客户端可以根据内容展示页面、数据等。

这是一个基本的 Servlet 工作流程，实际中可能涉及更多的细节和处理步骤。通过编写 Servlet，你可以处理来自客户端的请求，生成动态的 Web 内容，并向客户端发送响应。

# 6.Servlet的实现方式

- 方式一：继承HttpServlet类

  - ```java
    package com.xxxx.servlet;
    import javax.servlet.http.HttpServlet;
    public class Servlet01 extends HttpServlet{
    	...
    }
    ```

- 方式二：继承了GenericServlet类

  - ```java
    package com.example.demo;
    
    import javax.servlet.GenericServlet;
    import javax.servlet.ServletException;
    import javax.servlet.ServletRequest;
    import javax.servlet.ServletResponse;
    import javax.servlet.annotation.WebServlet;
    import java.io.IOException;
    
    @WebServlet("/sr03")
    public class Servlet03 extends GenericServlet {
        @Override
        public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
            System.out.println("继承了GenericServlet类");
        }
    }
    
    ```

- 方式三：实现Servlet接口

  - ```java
    package com.example.demo;
    
    import javax.servlet.*;
    import javax.servlet.annotation.WebServlet;
    import java.io.IOException;
    
    @WebServlet("/sr02")
    public class Servlet02 implements Servlet {
        @Override
        public void init(ServletConfig servletConfig) throws ServletException {
    
        }
    
        @Override
        public ServletConfig getServletConfig() {
            return null;
        }
    
        @Override
        public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
            System.out.println("实现Servlet接口。。。");
        }
    
        @Override
        public String getServletInfo() {
            return null;
        }
    
        @Override
        public void destroy() {
    
        }
    }
    ```

# 7.servlet生命周期

**init**方法，在Servlet实例创建之后执行（证明该Servlet有实例创建了）--->服务器自动调用（只调用一次）

```java
  public void init(ServletConfig servletConfig) throws ServletException {
    ...
    }
```

**service**方法，每次有请求到达某个Servlet方法时执行，用来处理请求（证明该Servlet进行服务了）

```java
    public void service(HttpServletRequest request, HttpServletResponse response) throws IOException {
    ...
    }
```

**destory**方法，Servlet实例销毁时执行（证明Servlet的实例被销毁了）--->服务器自动调用（只调用一次）

```java
    public void destroy() {
      ...
    }
```

**Servlet的生命周期，简单的概括这就分为四步：servlet类加载-->实例化-->服务-->销毁**

Tomcat与Servlet是如何工作的？

1. Web Client向Servlet容器（Tomcat）发出Http请求
2. Servlet容器接收Web Client的请求
3. Servlet容器创建一个HttpServletRequest对象，将Web Client请求的信息封装到这个对象中
4. Servlet容器创建一个HttpServletResponse对象
5. Servlet容器调HttpServlet对象service方法，把Request与Response作为参数，传给HttpServlet
6. HttpServlet调用HttpServletRequest对象的有关方法，获取Http请求信息
7. HttpServlet调用HttpServletResponse对象的有关方法，生成响应数据
8. Servlet容器把HttpServlet的响应结果传给Web Client

# 8.HttpServletRequest对象

`HttpServletRequest` 是 Java Servlet API 中的一个接口，用于表示客户端发送到服务器的HTTP请求。它提供了访问请求头、请求参数、请求方法、请求路径等信息的方法，以及读取请求体中的数据。

通过 `HttpServletRequest`，你可以获取有关客户端请求的各种信息，包括但不限于以下内容：

1. **请求头信息：** 可以获取请求的各种头信息，如User-Agent、Content-Type、Accept-Language等。
2. **请求参数：** 可以获取通过URL查询参数、表单提交或其他方式传递到服务器的参数值。
3. **请求方法：** 可以获取HTTP请求的方法，如GET、POST、PUT、DELETE等。
4. **请求路径：** 可以获取请求的URI（Uniform Resource Identifier）或URL，用于确定请求的资源路径。
5. **Session 和 Cookie：** 可以获取与会话和Cookie相关的信息。
6. **请求体数据：** 如果是 POST 或 PUT 请求，可以通过 `getInputStream` 或 `getReader` 方法读取请求体中的数据。
7. **文件上传：** 可以获取通过表单上传的文件信息。
8. **其他属性：** 可以获取和设置请求的一些属性，如字符编码、内容长度等。

为了使用 `HttpServletRequest`，你需要在 Servlet 中引入相应的包，并在 `doGet` 或 `doPost` 等方法中使用方法来获取请求信息。这样，你就可以根据客户端发送的请求信息，进行相应的处理和响应。

- 常用方法：

| 方法                            | 说明                                              |
| ------------------------------- | ------------------------------------------------- |
| getRequestURL()                 | 返回请求的完整URL，包括协议、主机、端口和路径。   |
| getRequestURI()                 | 返回请求的URI（不包括查询参数部分）。             |
| getContextPath()                | 获取当前 Web 应用程序的上下文路径（Context Path） |
| getMethod()                     | 返回请求的HTTP方法，如 "GET"、"POST" 等。         |
| getQueryString()                | 返回客户端请求中的查询字符串。                    |
| **getParameter(String name)**   | **根据参数名获取请求中的参数值。**                |
| getParameterValues(String name) | 获取客户端请求中指定参数名的多个参数值。          |
| getParameterMap()               | 返回所有查询参数的映射。                          |

- 请求乱码问题

  - GET请求：不会乱码

  - POST请求：会乱码，通过设置服务器解析编码的格式---->request.setCharacterEncoding("UTF-8")		 

    ※乱码原因：由于在解析过程中默认使用的编码方式为ISO-8859-1（此编码不支持中文），所以解析时一					 定会出现乱码

    ```java
    @WebServlet("/processForm")
    public class FormProcessorServlet extends HttpServlet {
        protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            // 设置请求的字符编码为UTF-8
            request.setCharacterEncoding("UTF-8");
    
            // 获取表单提交的数据
            String name = request.getParameter("name");
            String age = request.getParameter("age");
    
            // 在响应中显示获取到的数据
            response.setContentType("text/html;charset=UTF-8");
            PrintWriter out = response.getWriter();
            out.println("<html><body>");
            out.println("Name: " + name + "<br>");
            out.println("Age: " + age + "<br>");
            out.println("</body></html>");
        }
    }
    
    ```

- 请求转发

  请求转发，是一种服务器的行为，当客户端请求到达后，服务器进行转发，此时会将请求对象进行保存，地址栏中的URL地址不会改变，得到响应后，服务器再将响应发送给客户端，从始至终只有一个请求发出。

  ※可以让请求从服务端跳到客户端（或跳转到指定Servlet）

  特点：

  - 服务端行为
  - 地址栏不发生改变
  - 从始至终只有一个请求
  - request数据可以共享

  实现方式如下，达到多个资源协同响应的效果。

  ```java
  //请求转发跳转到Servlet
  request.getRequestDispatcher(”url”).forward(request,response); //目标servlet的地址
  
  //请求转发跳转到jsp页面
  request.getRequestDispatcher(”url”).forward(request,response); //目标Jsp的地址
  
  //请求跳转到HTML用
  request.getRequestDispatcher(”url”).forward(request,response); //目标Html的地址
  ```

- request作用域		

  - 通过该对象可以在一个请求中传递数据 	

    **作用范围**：在一次请求中有效，即服务器跳转有效（请求转发跳转时有效）	

    - 设置域对象内容：request.setAttribute("name","chris");
    - 获取域对象内容：request.getAttribute("name")
    - 删除域对象内容：request.removeAttribute("name")

    **※请求转发到servlet或jsp，可通过域对象传递数据**

# 9.HttpServletResponse对象

`HttpServletResponse` 是 Java Servlet API 提供的一个接口，用于生成服务器响应给客户端。它提供了一系列方法，可以设置响应头、状态码、响应内容等，从而构建完整的 HTTP 响应。

以下是一些常用的 `HttpServletResponse` 方法和其作用：

1. **设置响应状态码：**
   - `setStatus(int sc)`：设置响应的状态码，例如 200 表示成功，404 表示未找到资源。
2. **设置响应头：**
   - `setHeader(String name, String value)`：设置指定名称的响应头为给定的值。
   - `addHeader(String name, String value)`：添加一个具有指定名称和值的响应头。
3. **设置响应内容类型：**
   - `setContentType(String type)`：设置响应的内容类型，例如 "text/html" 或 "application/json"。
4. **获取输出流：**
   - `getWriter()`：获取用于写入字符数据的 PrintWriter 对象。
   - `getOutputStream()`：获取用于写入字节数据的 ServletOutputStream 对象。
5. **设置重定向：**
   - `sendRedirect(String location)`：发送重定向响应，将客户端重定向到指定的 URL。
6. **设置响应内容：**
   - 使用 `getWriter()` 或 `getOutputStream()` 获取输出流，然后将内容写入流中。
7. **设置缓存控制：**
   - `setDateHeader(String name, long date)`：设置指定名称的日期头字段的值。
   - `setExpires(long expires)`：设置响应过期时间。
   - `setCacheControl(String control)`：设置缓存控制头。
8. **设置字符编码：**
   - `setCharacterEncoding(String charset)`：设置响应字符编码。
9. **设置跨域头部：**
   - `addHeader("Access-Control-Allow-Origin", "*")`：设置允许跨域请求。

- 响应数据

​		接收到客户端请求后，可以通过HttpServletResponse对象直接进行乡响应，响应时需要获取输出流。

​		有两种形式：

​						getWriter()  获取字符流（只能响应字符流）

​						getOutputStream()	获取字节流（能响应一切数据）

​				响应回的数据到客户端被浏览器解析。

​				**注意：两者不能同时使用。**

```java
//获取字符流
PrintWriter writer = response.getWriter();
//输出数据
writer.write("Hello,");
```

```java
//字节输出流
ServletOutputStream out = response.getOutputStream();
//输出数据
out.write("<h2>hi</h2>".getBytes());
```

- 响应乱码问题

  - getWriter() 的字符乱码
    - 服务端的默认编码方式为ISO-8859-1（此编码不支持中文）
  - getOutputStream() 的字节乱码
    - 因为本身传输的是字节，所以可能出现乱码

  解决方案：

  ```java
  //指定客户端和服务器使用的编码方式一只
  response.setContentType("text/html;charset=utf-8");
  ```

- 重定向

  重定向是一种服务器指导，客户端的行为。客户端发出的第一个请求，被服务器处理后，服务器会进行响应，在响应的同时，服务器会给客户端一个新的地址（下次请求的地址response.sendRedirect("index.jsp");），当客户端接收到响应后，会立刻，马航，自动根据服务器给的新地址发起第二个请求，服务器接收请求并做出响应，重定向完成。

  从描述中可以看出重定向当中有连个请求，并且属于客户端行为。

  ```java
  // 重定向跳转到index.jsp
  response.sendRedirect("index.jsp");
  ```

  特点：

  - 服务端知道，客户端行为
  - 存在两次请求
  - 地址栏回发生改变
  - request对象不共享

10.请求转发与重定向的区别

请求转发和重定向比较

请求转发：req.getRequestDispatcher(”url”).forward(req,res)

重定向：response.sendRedirect("url");

| 请求转发                        | 重定向                          |
| ------------------------------- | ------------------------------- |
| 一次请求，数据在request域中共享 | 两次请求，request域中数据不共享 |
| 服务器端行为                    | 客户端行为                      |
| 地址栏不发生变化                | 地址栏发生变化                  |
| 绝对地址定位到站点后            | 绝对定制可写到 http://          |

两者都可进行跳转，根据实际需求选取即可。

# 10.Cookie对象

有一个专门操作Cookie的类 javax.servlet.httpp.Cookie 随着服务端的响应发送给客户端，保存在浏览器。

当下次再询问服务器把Cookie再带回服务器。

Cookie的格式：键值对用“=”连接，多个键值对间通过“;“隔开

- Cookie的创建和发送

```java
Cookie cookie = new Cookie("name","chris");
resp.addCookie(cookie);
```

- Cookie的获取

```java
Cookie[] cookies = req.getCookies();
if(cookies != null && cookies.length>0){
     for(Cookie cookie : cookies){
         String name = cookie.getName();
         String value = cookie.getValue();
         System.out.println("key: "+name+"  value: "+ value);
      }
}
```

- Cookie的设置到期时间

在 Servlet 中，Cookie 的到期时间可以通过设置 `setMaxAge(int maxAge)` 方法来指定。`setMaxAge` 方法接受一个整数值，表示从当前时间开始的秒数，指定了 Cookie 的存活时间。以下是一些常用的取值情况：

1. **正数值：** 设置为正数值表示 Cookie 将在指定的秒数后过期。例如，`setMaxAge(3600)` 表示 Cookie 会在一个小时后过期。

2. **0：** 设置为 0 表示立即删除 Cookie，相当于删除该 Cookie。

3. **负数值：** 设置为负数值表示该 Cookie 将在客户端会话结束时过期。当客户端关闭浏览器时，会话结束，Cookie 将被删除。

   ```java
   // 创建一个 Cookie
   Cookie cookie = new Cookie("username", "john_doe");
   
   // 设置 Cookie 的到期时间为 1 小时后
   cookie.setMaxAge(3600);
   
   // 添加 Cookie 到响应中
   response.addCookie(cookie);
   ```

- Cookie的注意点

  - Cookie保存中当前浏览器中

  - Cookie存中文问题（不建议存中文）

  - 同名Cookie问题：如果服务端发送重复的Cookie那么会覆盖原有的Cookie

  - 浏览器存放Cookie的数量（4KB）

    不同的浏览器对Cookie也有限定，Cookie的存储是有上限的。Cookie是存储在客户端（浏览器）的，而且一般是由服务端创建和设定。后期结合Session来实现回话跟踪。

- Cookie的路径

  在 Servlet 中，通过设置 Cookie 的路径属性，你可以控制哪些 URL 能够访问该 Cookie。这个路径属性决定了哪些路径下的请求可以访问到该 Cookie。路径属性对于控制 Cookie 的作用范围非常重要。
  
  Cookie 路径属性的设置方法是使用 `setPath(String path)` 方法。这个方法接受一个字符串参数，表示 Cookie 的路径。默认情况下，如果不设置路径，Cookie 的路径会自动设置为当前请求的路径。
  
  以下是一些示例来说明 Cookie 路径的作用：
  
  1. **设置路径为根路径 `/`：** 这意味着任何在应用程序的根路径下的 URL 都可以访问该 Cookie。
  
     ```java
     javaCopy code
     Cookie cookie = new Cookie("username", "john_doe");
     cookie.setPath("/");
     response.addCookie(cookie);
     ```
  
  2. **设置路径为子路径：** 你可以将 Cookie 的路径设置为特定的子路径，以限制哪些 URL 可以访问它。
  
     ```java
     javaCopy code
     Cookie cookie = new Cookie("username", "john_doe");
     cookie.setPath("/app"); // 只有在 /app 路径下的 URL 可以访问该 Cookie
     response.addCookie(cookie);
     ```
  
  注意，路径属性并不是真正的安全机制，它只是限制了哪些 URL 可以访问到 Cookie。如果某个路径下存在恶意代码，它仍然可以访问到合法路径下的 Cookie。
  
  通常情况下，设置 Cookie 的路径为根路径 `/` 可以使 Cookie 在整个应用程序范围内可见，而设置特定的子路径可以限制 Cookie 的可见范围。
  
  需要注意的是，如果某个路径设置了 Cookie，那么该路径的所有子路径都可以访问该 Cookie。例如，如果设置了路径为 `/app`，那么 `/app/page1`、`/app/page2` 等路径下的 URL 都可以访问该 Cookie。