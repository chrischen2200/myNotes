# JSP和JSTL

# 1.JSP基础语法

## 1-1.JSP中一般有两种类型的注释

1. 显式注释

   能够在客户端查看的注释

   - 继承HTML风格的注释  `&lt!--HTML注释--&gt`

     <!-- HTML注释 -->

2. 隐式注释

   不能在客户端查看的注释

   - JSP自己的注释 `%lt!--HTML注释--%gt`

     <%-- JSP自己的注释 --%>

     ```jsp
     <%-- Java脚本段 --%>
     ```

   - 继承Java风格的注释

     //单行注释

     /* 多行注释 */

     ```jsp
     <%
     //这是单行注释
     /* 这是多行注释 */
     %>
     ```

## 1-2.scriptlet

在 JSP（JavaServer Pages）中，scriptlet 是一种允许在页面中嵌入 Java 代码的方式。它允许在页面中直接编写 Java 代码来处理业务逻辑和生成动态内容。以下是 scriptlet 的三种不同的格式：

1. **Java 代码块形式（<% ... %>）：** 这是最常见的 scriptlet 形式，它使用 `<%` 开始标记和 `%>` 结束标记，之间是 Java 代码。在这个块中，你可以编写任何合法的 Java 代码，如变量声明、条件判断、循环等。

   示例：

   ```
   jspCopy code
   <% 
      String name = "John";
      out.println("Hello, " + name);
   %>
   ```

2. **Java 表达式形式（<%= ... %>）：** 这种形式用于在页面中输出 Java 表达式的值。它使用 `<%=` 开始标记和 `%>` 结束标记，之间是一个 Java 表达式。表达式的值会被计算并输出到页面。

   示例：

   ```
   jspCopy code
   <p>Today's date: <%= new java.util.Date() %></p>
   ```

3. **声明（<%! ... %>）：** 这种形式用于在页面中声明成员变量和方法，这些成员变量和方法在整个页面范围内都可用。它使用 `<%!` 开始标记和 `%>` 结束标记，之间是 Java 代码。

   示例：

   ```
   jspCopy code
   <%!
      private int count = 0;
   
      public void incrementCount() {
          count++;
      }
   %>
   ```

虽然 scriptlet 可以实现页面中的动态逻辑，但在现代的 Web 开发中，推荐使用 JSTL 标签和 EL 表达式来实现逻辑和显示的分离，以提高页面的可维护性和可读性。

# 2.JSP的指令标签

## 2-1.include静态包含

用于包含其他文件的内容，可以是 JSP 文件、HTML 文件等。

```jsp
<%@ include file="header.jsp" %> 　<!--相对路径-->
```

特点：

1. 将内容进行了直接的替换
2. 静态包含智慧生成一个源码文件，最终的内容全部在_jspService方法体中（源码文件中）
3. 不能出现同名变量
4. 运行效率高一点点。耦合性较高，不够灵活。

## 2-2.include动态包含

`<jsp:include>` 是 JSP（JavaServer Pages）中的动态包含方式，它允许你在运行时根据条件来包含其他 JSP 文件的内容。被包含的内容将在请求时动态地合并到当前 JSP 页面中。

```jsp
//单标签
<jsp:include page="included.jsp" />   //page的值可以动态传值

//双标签
<jsp:include page="included.jsp" ><jsp:include>  //page的值可以动态传值
```

特点：

1. 动态包含险挡雨方法的调用

2. 动态包含会生成多个源码文件

3. 可以定义同名变量

4. 效率高，耦合度低

   注意：当动态包含不需要传递参数时，include双标签之间不要有任何内容，包含换行和空格

   ```jsp
   <jsp:include page="included.jsp">
         <jsp:param name="name" value="John" />
         <jsp:param name="age" value="<%=str%>" />  //name属性不支持表达式，value属性支持表达式
   </jsp:include>
   ```
   
   获取参数：request.getParameter("age")  通过指定参数名获取参数值

# 3.JSP的四大域对象（内存对象）

## 3-1.四种属性范围

在jsp中提供了四种属性的保存范围，所谓的属性保存范围，指的就是一个设置的对象，可以再多少个页面中保存并可以继续使用

1. pageContxet范围

   pageContxet：只在一个页面中曹村属性，跳转之后无效

2. requust范围：

   request：只在一次请求中保存，服务器跳转之后依然有效

3. session范围：

   session：在一次会话范围中，无论何种跳转都可以使用

4. application范围：

   application：在整个服务器上保存

   | 方法                                            | 类型 | 描述                 |
   | ----------------------------------------------- | ---- | -------------------- |
   | public void setAttribute(String name, Object o) | 普通 | 设置属性的名称及内容 |
   | public Object getAttribute(String name)         | 普通 | 根据属性名称取属性值 |
   | public void removeAttribute(String name)        | 普通 | 删除指定的属性       |

## 3-2.验证属性范围的特点

1. page

   本页面取得，服务器端跳转（jsp:forward）后无效

2. **request**（使用居多）

   服务器跳转有效，客户端跳转无效

   如果是客户端跳转，则相当于发出了2次请求，那么第一次的请求将不存在了；如果希望不管是客户端还是服务器调转，都能保存的话，就需要继续扩大范围。

3. session

   无论客户端还是服务器端都可以取得，但是现在需要重新开启一个新的浏览器，则无法取得之前设置的seeeion了，应为每一个session值保存在当前的浏览器单中，并在相关的页面取得。

   对于服务器而言，每一个连接到它的客户端都是一个session

   如果想要让属性设置一次之后，不管是否是新的浏览器打开都能取得则可以使用application

4. application（servlet中的servletContext对象）

   所有的application属性直接保存在服务器上，所有的用户（每一session）都可以直接访问取得

   只要是通过application设置的属性，则所有的session都可以取得，标识公共的内容，但是如果此时服务器重启了，则无法取得了，应为关闭服务器后，所有的属性都消失了，所以需要重新设置。

**问：使用哪个范围呢？**

**答：在合理范围尽可能小**

补充：JSP中跳转方式

1. 服务端跳转		

   ```jsp
   <jsp:forward page="跳转的页面地址"></jsp:forward>
   ```

2. 客户端跳转	

   ```html
   <a href="跳转的页面地址">跳转</a>
   ```


# 4.EL表达式的使用

## 4-1.EL表达式语法

EL（Expression Language）表达式是一种用于在 JSP（JavaServer Pages）页面中访问和操作对象的简洁语法。EL 表达式使得在页面中获取和操作 JavaBean 属性、集合元素、Map 值等变得更加方便和易读。

EL 表达式的语法是 `${expression}`，其中 `expression` 是要计算的表达式。EL 表达式可以嵌套，也可以在标签属性中使用。

```
${expression}
```

**EL表达式一般操作的都是域对象中的数据，操作不了局部变量。**

**域对象的概念在JSP中一共有四个：pageContext,request,session,application;**

```
//获取指定域的对象数据：域.变量名
${pageScope.expression}
```

**范围依次是，本页面，一次请求，一次会话，整个应用程序。**

当需要指定从某个特定的域对象中查找数据时可以使用四个域对象对应的空间对象，分别是：pageScope，requestScope，sessionScope，applicationScope。

而EL默认的查找方式为从小到大查找，找到即可。当域对象全找完了还未找到则返回空字符串“”。

## 4-2.EL表达式的使用

- 设置域对象中的数据

  ```jsp
  <%
      String pageTitle = "Page Scope Example";
      pageContext.setAttribute("pageTitle", pageTitle); //设置页面作用域数据
  
      String message = "Hello, Request Scope!";
      request.setAttribute("message", message); //设置请求作用域数据
  
      String username = "john123";
      session.setAttribute("username", username); //设置会话作用域数据
  
      int visitorCount = 1000;
      application.setAttribute("visitorCount", visitorCount); //设置应用程序作用域数据
      %>
  ```

- 获取域对象的值

  ```jsp
  <%--获取域对象中的数据：默认的查找方式为从小到大查找，找到即可。当域对象全找完了还未找到则返回空字符串“”-->
  ${uname}
  ```

- 获取指定域对象的值

  ```jsp
  ${pageScope.pageTitle}
  ${requestScope.message}
  ${sessionScope.username}
  ${applicatoinScope.visitorCount}
  ```

- 其他常用方法

  1. 访问 JavaBean 属性：

     ```jsp
     <c:set var="person" value="${new com.example.Person()}" />
     <p>Name: ${person.name}</p>
     <p>Age: ${person.age}</p>
     ```

  2. 访问集合元素：

     ```jsp
     <c:set var="numbers" value="${[1, 2, 3, 4, 5]}" />
     <p>First Number: ${numbers[0]}</p>
     <p>Last Number: ${numbers[4]}</p>
     ```

  3. 访问 Map 值：

     ```jsp
     <c:set var="student" value="${{'name': 'John', 'age': 20}}" />
     <p>Name: ${student['name']}</p>
     <p>Age: ${student['age']}</p>
     ```

  4. 使用算术和逻辑运算符：

     ```jsp
     <p>Sum: ${2 + 3}</p>
     <p>Is 5 greater than 3? ${5 > 3}</p>
     ```

  5. 调用方法：

     ```jsp
     <c:set var="text" value="${'Hello, World!'}" />
     <p>Length of Text: ${text.length()}</p>
     ```

  6. 使用条件运算符：

     ```jsp
     <c:set var="score" value="${85}" />
     <p>Grade: ${score >= 60 ? 'Pass' : 'Fail'}</p>
     ```

## 4-2.empty

在 EL（Expression Language，表达式语言）中，`${empty}` 是一个用于判断值是否为空的内置函数。它用于检查一个值是否为 `null` 或者为空（例如空字符串、空集合等）。

以下是 `${empty}` 在 EL 表达式中的使用示例：

```jsp
<c:if test="${empty username}">
    <!-- 如果 username 为空，则执行此代码块 -->
    用户名为空。
</c:if>

<c:if test="${not empty books}"> 
    <!-- 如果 books 不为空，则执行此代码块 -->
    有可用的图书。
</c:if>
```

`${not empty}` 和 `${!empty}` 都是用于判断值是否不为空的 EL（Expression Language，表达式语言）内置函数。它们在功能上是等效的，可以用来检查一个值是否不为 `null` 或者不为空（例如非空字符串、非空集合等）。

## 4-3.EL运算

比较两个值是否相等，返回true或false

**== 或 eq**

其他运算与java相等

# 5.JSTL

JSTL（JavaServer Pages Standard Tag Library）是 JavaServer Pages（JSP）的一个标准标签库，它提供了一组标签和函数，用于在 JSP 页面中执行常见的操作，如条件判断、循环、格式化、数据库访问等。使用 JSTL 可以减少在 JSP 页面中编写 Java 代码的需要，使页面更加简洁和易于维护。

JSTL 主要包含以下几个标签库：

1. **核心标签库（c 标签库）：** 提供了条件判断、循环、字符串操作等功能。
2. **格式化标签库（fmt 标签库）：** 提供了日期、数字和消息的格式化功能。
3. **XML 标签库（x 标签库）：** 用于处理 XML 数据。
4. **SQL 标签库（sql 标签库）：** 用于进行数据库访问。
5. **函数标签库（fn 标签库）：** 提供了一些常用的函数。

## 5-1.JSTL的使用

1. 下载JSTL所需要的jar包（standar.jar 和jstl.jar)

   从Apache的标准标签库中下载的二进包（

   官方下载地址：http://archive.apache.org/dist/jakarta/taglibs/standard/binaries/

   ```
   jakarta-taglibs-standard-1.1.2.zip 
   ```

2. 在项目web目录下的WEB-INF中创建lib目录，将jar拷贝进去

3. 选择“File”，再选择“Project Structure”，

4. 选择”Mdules“再选择右侧的“Dependecncies”，选择右侧的“+“号，将对应的lib目录加载进来

5. 在需要使用标签库的JSP页面通过tablib标签引入指定库

   ```jsp
   <%@ taglib prefix="" uri="" %>
   ```

   例如：

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   ```

## 5-2.核心标签库

条件动作指令用于处理页面的输出结果依赖于某些输入值的情况，在Java中是利用if，if..else和switch语句来进行处理的。在JSTL中也有4个标签可以执行条件式动作指令：if，choose，when和otherwise。

### 5-2-1.if标签

if标签先对某个条件进行测试，如果该条件运算结果为true，则处理它的主体内容，测试结果保存一个Boolean对象中，并创建一个限域变量来引用Boolean对象。可以利用var属性设置限域变量名，利用scope属性来指定其作用范围。

1. 语法格式

   ```jsp
   　<c:if test="<boolean>" var="<string>" scope="<string>">
   　	 ...
   　</c:if>
   ```

2. 属性

   if标签有如下属性：

   | 属性  | 描述                                                         | 是否必要 | 默认值 |
   | ----- | ------------------------------------------------------------ | -------- | ------ |
   | test  | 条件判断，操作的是域对象，接收返回结果是boolean类型的值      | 是       | 无     |
   | var   | 用于存储条件结果的变量（限域变量名）                         | 否       | 无     |
   | scope | var属性的作用域<br />可取值：page｜request｜session｜application | 否       | page   |

### 5-2-2.choose，when和otherwise标签

choose和when标签的作用与java中的switch和case的关键字相似，用于在众多选项中做出选择。也就是说：他们为互相排斥的条件式执行提供相关内容。

switch语句中有case，而choose标签中对应有when，switch语句中有default，而choose标签中有otherwise。

1. 语法格式

   ```jsp
   <c:choose>
       <c:when test="${condition1}">
           <!-- 当 condition1 为真时执行 -->
           Condition 1 is true.
       </c:when>
       <c:when test="${condition2}">
           <!-- 当 condition2 为真时执行 -->
           Condition 2 is true.
       </c:when>
       <c:otherwise>
           <!-- 所有条件都不满足时执行 -->
           No conditions are true.
       </c:otherwise>
   </c:choose>
   ```

2. 属性

   - choose标签没有属性

   - when标签只有一个test属性

   - otherwise标签没有属性

     注意：

     - choose标签和otherwise标签没有属性，而when标签必须有一个test属性
     - choose标签中必须包含至少一个when标签，可以没有otherwise标签
     - otherwise标签必须设置在最后一个when标签之后
     - choose标签中只能设置when标签与otherwise标签
     - when标签与otherwise标签可以嵌套其他标签
     - otherwise标签会在所有的when标签不执行时才会执行

### 5-2-3.foreach标签（迭代标签）

`<c:forEach>` 是 JSTL（JavaServer Pages Standard Tag Library）核心标签库（c 标签库）中的一个标签，用于在 JSP 页面中进行循环遍历操作。它可以用于遍历集合、数组、范围等，并在循环中执行指定的代码块。

1. 语法格式

   ```jsp
   <c:forEach 
   	items="<object>"
   	begin="<int>"
   	end="<int>"
   	step="<string>"
   	var="<string>"
   	varStatus="<string>"
   	>
       <!-- 循环体内的代码 -->
   </c:forEach>
   ```

2. 属性

   | 属性      | 描述                                                         | 是否必要 | 默认值       |
   | --------- | ------------------------------------------------------------ | -------- | ------------ |
   | items     | 要被循环的数据                                               | 否       | 无           |
   | begin     | 开始的元素（0=第一元素，1=第二个元素）                       | 否       | 0            |
   | end       | 最后一个元素（0=第一元素，1=第二个元素）                     | 否       | Last element |
   | step      | 每一次迭代的步长                                             | 否       | 1            |
   | var       | 代表当前条目的变量名称                                       | 否       | 无           |
   | varStatus | 代表循环状态的变量名称<br />index:当前这次迭代从0开始的迭代索引<br />count:当前这次迭代从1开始迭代计数<br />first:用来表明当前这论迭代是否为第一次迭代的标志<br />last:用来表明当前这论迭代是否为最后一次迭代的标志 | 否       | 无           |

3. 用法

   1. 迭代主题内容多次

      ```jsp
      <c:forEach begin="开始数" end="结束数" step="间隔数" var="限域变量名"
      </c:forEach>
      //相当于Java中for ...int
      for(int i=0;i<10;i++){}
      ```

      ```jsp
          <c:forEach var="i" begin="1" end="10" step="2">
               ${i} &nbsp;
          </c:forEach>
      ```

   2. 循环

      ```jsp
      <c:forEach items="要被循环的数据" var="限域变量名"
      </c:forEach>
      //相当于Java中foreach
      for(String str : list){}
      ```

      ```jsp
      <c:set var="fruits" value="${['apple', 'banana', 'orange']}" />
      
      <ul>
          <c:forEach var="fruit" items="${fruits}">
              <li>${fruit}</li>
          </c:forEach>
      </ul>
      ```

   ### 5-2-4.其他标签

   1. **`<c:out>` 标签：** 用于输出表达式的值，可以选择是否转义特殊字符。

      ```jsp
      <c:out value="${message}" escapeXml="true" />
      //escapeXml="true" 属性指定了是否要对输出的内容进行 XML 转义，将特殊字符（例如 <、>、&、"、' 等）转换为对应的实体编码，以避免 HTML 或 XML 注入攻击。
      ```

   2. **`<c:set>` 标签：** 用于设置变量的值，并将其存储在指定作用域中。

      ```jsp
      <c:set var="count" value="10" scope="request" />
      ```

   3. **`<c:import>` 标签：** 用于包含其他页面的内容。

      ```jsp
      <c:import url="/path/to/another/page.jsp" />
      ```

   4. **`<c:redirect>` 标签：** 用于重定向到另一个页面。

      ```jsp
      <c:redirect url="/path/to/another/page.jsp" />
      ```

## 5-3.格式化标签库

JSTL（JavaServer Pages Standard Tag Library）格式化标签库（fmt 标签库）提供了一组标签，用于在 JSP 页面中进行日期、数字和消息的格式化。常用的有：formatNumber，formatDate，parseNumber及parseDate。

### 5-3-1.formatNumber标签

`<fmt:formatNumber>` 是 JSTL（JavaServer Pages Standard Tag Library）格式化标签库（fmt 标签库）中的一个标签，用于格式化数字，如货币、百分比等。

**将数值型转化成指定格式的字符串。**

1. 语法格式

   ```jsp
   <fmt:formatNumber 
   	value="<string>"
     type="<string>"
     var="<string>"
     scope="<string>"
     />
   ```

2. 属性

   | 属性  | 描述                                                         | 是否必要 | 默认值        |
   | ----- | ------------------------------------------------------------ | -------- | ------------- |
   | value | 要显示的数字                                                 | 是       | 无            |
   | type  | NUMBER：数值类型<br />CURRENCY：货币类型<br />PERCENT：百分比类型 | 否       | Number        |
   | var   | 存储格式化数字的变量                                         | 否       | Print to page |
   | scope | var属性的作用域                                              | 否       | page          |

   注意：

   - 如果设置levar属性，则格式化后的结果不会输出，需要通过el表达式获取var对应的限域变量名
   - 默认的类型（type）的取值number。可取值：number数值型，percent百分比类型，currency货币型

3. 示例

   ```jsp
   <fmt:formatNumber value="12345.6789"  type="number" var="num"/>${num}
   <fmt:formatNumber value="0.456" type="percent" />
   <fmt:formatNumber value="12345.6789" type="currency" />
   
   //设置时区
   <fmt:setLocale value="en_US" />
   <fmt:formatNumber value="12345.6789" type="currency" />
   
   //直接输出指定货币
   <fmt:formatNumber value="12345.6789" type="currency" currencyCode="USD" />
   ```

### 5-3-2.formatDate标签

`<fmt:formatDate>` 是 JSTL（JavaServer Pages Standard Tag Library）格式化标签库（fmt 标签库）中的一个标签，用于格式化日期和时间。

1. 语法格式

   ```jsp
   <fmt:formatDate
   	value="<string>"
   	type="<string>"
   	dateStyle="<string>"
   	timeStyle="<string>"
   	pattern="<string>"
   	timeZone="<string>"
   	var="<string>"
   	scope="<string>"/>
   ```

2. 属性

   | 属性        | 描述                                                         | 是否必要 | 默认值     |
   | ----------- | ------------------------------------------------------------ | -------- | ---------- |
   | value       | 要显示的日期                                                 | 是       | 无         |
   | type        | DATE：日期型 年月日<br />TIME：时间型 时分秒<br />BOTH：日期时间型 | 否       | date       |
   | dateSytle   | FULL，LONG，MEDIUM，SHORT活DEFAULT                           | 否       | default    |
   | timeStlye   | FULL，LONG，MEDIUM，SHORT活DEFAULT                           | 否       | default    |
   | **pattern** | **自定义格式模式**（ y M d H m s）                           | 否       | 无         |
   | timeZone    | 显示日期的时区                                               | 否       | 默认时区   |
   | var         | 存储格式化日期的变量名                                       | 否       | 显示在页面 |
   | scope       | 存储格式化日期变量的范围                                     | 否       | page       |

3. 示例

   ```jsp
   <fmt:formatDate value="${now}" pattern="yyyy-MM-dd" />
   <fmt:formatDate value="${timestamp}" pattern="yyyy-MM-dd HH:mm:ss" />
   ```

### 5-3-3.其他常用标签

1. parseNumber标签

   `<fmt:parseNumber>` 是 JSTL（JavaServer Pages Standard Tag Library）格式化标签库（fmt 标签库）中的一个标签，用于将字符串解析为数字类型。它可以帮助您在 JSP 页面中将字符串转换为数字，并进行格式化显示。

   以下是 `<fmt:parseNumber>` 标签的基本语法：

   ```jsp
   <fmt:parseNumber var="result" value="${numberString}" [type="formatType"] [pattern="formatPattern"] />
   ```

   常用的属性包括：

   - `var`：将解析后的数字存储到指定的变量中。
   - `value`：要解析的字符串，可以是 EL（Expression Language）表达式。
   - `type`：可选，指定解析的类型，可以是 "number"、"currency" 或 "percent"。
   - `pattern`：可选，指定自定义的解析模式，用于解析特定格式的数字字符串。

   以下是一个 `<fmt:parseNumber>` 标签的使用示例：

   ```jsp
   <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
   
   <c:set var="numberString" value="12345.6789" />
   
   <fmt:parseNumber var="result" value="${numberString}" type="number" />
   
   <p>Parsed Number: ${result}</p>
   ```

   在上面的示例中，`${numberString}` 是一个字符串，通过 `<fmt:parseNumber>` 标签将它解析为数字，并将解析后的结果存储在变量 `${result}` 中。然后，使用 `${result}` 就可以在页面中显示解析后的数字。

   通过使用 `<fmt:parseNumber>` 标签，您可以在 JSP 页面中将字符串转换为数字，并根据需要进行格式化和显示。在使用这个标签之前，确保您在 JSP 页面的开头引入以下声明：

   ```jsp
   <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
   ```

2. parseDate标签

   `<fmt:parseDate>` 是 JSTL（JavaServer Pages Standard Tag Library）格式化标签库（fmt 标签库）中的一个标签，用于将字符串解析为日期类型。它可以将一个表示日期和时间的字符串解析为 Java 中的 `java.util.Date` 或 `java.sql.Date` 类型，以便在页面中进行操作和显示。

   以下是 `<fmt:parseDate>` 标签的基本语法：

   ```jsp
   <fmt:parseDate var="result" value="${dateString}" [pattern="formatPattern"] />
   ```

   常用的属性包括：

   - `var`：将解析后的日期存储到指定的变量中。
   - `value`：要解析的日期字符串，可以是 EL（Expression Language）表达式。
   - `pattern`：可选，指定日期时间的解析模式，用于解析特定格式的日期时间字符串。

   以下是一个 `<fmt:parseDate>` 标签的使用示例：

   ```jsp
   <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
   
   <c:set var="dateString" value="2023-08-16" />
   
   <fmt:parseDate var="result" value="${dateString}" pattern="yyyy-MM-dd" />
   
   <p>Parsed Date: ${result}</p>
   ```

   在上面的示例中，`${dateString}` 是一个日期字符串，通过 `<fmt:parseDate>` 标签将其解析为日期，然后存储在变量 `${result}` 中，可以在页面中显示解析后的日期。

   通过使用 `<fmt:parseDate>` 标签，您可以在 JSP 页面中将日期字符串解析为日期对象，并在页面中操作和显示。在使用这个标签之前，确保您在 JSP 页面的开头引入以下声明：

   ```jsp
   <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
   ```



