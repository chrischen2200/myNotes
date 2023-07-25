## Mybatis项目（接口绑定方案）

#### 1.创建新的Maven项目

#### 2.配置pom.xml（各项依赖包的坐标，包含mybatis）

```xml
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.28</version>
</dependency>

<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.5.6</version>
</dependency>

<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>1.2.17</version>
</dependency>
```



#### 3.配置全局配置文件（/resources/mybatis.xml）

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="db.properties"></properties> //扫描目标包下的对象类：
    <typeAliases>
<!--        <typeAlias type="com.ohayoo.pojo.Book" alias="Book"></typeAlias>-->
        <package name="com.ohayoo.pojo"/>
    </typeAliases>
    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/ohayoo/mapper/BookMapper.xml"></mapper> //映射文件：
    </mappers>
</configuration>
```

​	扫描目标包下的对象类：

```xml
<properties resource="com.ohayoo.pojo"></properties>
```

​	映射文件：

```xml
<mapper resource="com/ohayoo/mapper/BookMapper.xml"></mapper>
```

#### 4.配置数据库属性文件（/resources/db.properties）

#### 5.日志配置文件（/resources/log4j.preoperties）

#### 6.创建实体类（Book.java）

#### 7.创建接口

```java
public interface BookMapper {
		public abstract List selectAllBooks();
}
```
#### 8.创建映射文件：要求：namespace取值必须是接口的全限定路径，标签中的id属性值必须和方法名对应 （看作是接口实现类）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ohayoo.mapper.BookMapper"> //全限定路径
    <select id="selectAllBooks" resultType="Book"> //id：接口的方法名  resultType：类的别名
        select * from t_book
        select * from t_book
    </select>
    <select id="selectOneBook" resultType="Book">
        select * from t_book where name =#{param1} and author =#{param2}
    </select>
    <insert id="insertBook" >
        insert into t_book(name,author,price) values(#{name},#{author},#{price})
    </insert>
    <update id="updateBook">
        update t_book set name=#{param2.name},author=#{param2.author},price=#{param2.price}  where name =#{param1}
    </update>
    <delete id="deleteBook">
        delete from t_book where name+#{param1}
    </delete>
</mapper>
```

#### 9.编写测试类（通过动态代理模式）

```java
public class Test {
    public static void main(String[] args) throws IOException {
      	//指定核心配置文件的路径
        String resource = "mybatis.xml";
      	//获取加载配置文件的输入流
        InputStream inputStream = Resources.getResourceAsStream*(resource);
      	//加载配置文件，创建工厂类
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
      	//通过工厂类获取一个会话
        SqlSession sqlSession = sqlSessionFactory.openSession();
				//动态代理模式 ？BookMapper mapper = BookMapper 实现类   （多态）
        BookMapper mapper = sqlSession.getMapper(BookMapper.class);
 				List list = mapper.selectAllBooks();
        for (Object o : list) {
            Book b = (Book) o;
            System.*out*.println(b.getName() + "----" + b.getAuthor());
        }
      
      	Book book = mapper.selectOneBook("西游记","吴承恩");
      	//关闭会话
      	sqlSession.close();

    }
}
```



备注：连接数据库需要驱动包：mysql



**==================================================================================================================================================================================================================**



## Spring框架

#### Ioc(控制反转)，DI（依赖注入）。属于同一件事情的两个名称。

​	创建对象的权利，或者是控制的位置，由JAVA代码转移到spring容器，由spring的容器控制对象的创建，就是控制反转。

​		**IOC/DI的原理：1.xml配置  		2.spring容器，放各种对象		3.JAVA程序**			



## Spring项目

#### 1.创建Maven项目，往pom.xml中添加依赖

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.23</version>
</dependency>
```

#### 2.创建一个类

#### 3.创建spring的配置文件：在src/main/resources/下新建applicationContext.xml

​	一个bean即是一个Java类的实例对象

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"    xsi:schemaLocation="http://www.springframework.org/schema/beans        https://www.springframework.org/schema/beans/spring-beans.xsd">  
  <!--id:bean的名称 class:类型全限定路径    -->
  <bean id="..." class="...">      <!-- collaborators and configuration for this bean go here -->    </bean>       <bean id="..." class="...">      <!-- collaborators and configuration for this bean go here -->   </bean>     	 
  <!-- more bean definitions go here --> 
  
</beans>
```

#### 4.创建spring容器（同时创建bean），从容器获取对象

```java
//创建Spring容器
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
//获取已经创建的对象
Book book =(Book)context.getBean("b") //b:bean的id
```

#### 5.属性注入

 设值注入：property的name属性的值和对象类的**setter方法名**相关

```xml
<bean id="b" class="com.ohayoo.pojo.Book">
    <property name="id" value="1"></property>
    <property name="name" value="项目驱动零起点学Java"></property>
</bean>
```

 构造注入：必须确保存在**有参构造器**(各个属性constructor-arg的name对应构造器参数名)

```xml
<bean id="b2" class="com.ohayoo.pojo.Book">
    <constructor-arg name="id" value="2"></constructor-arg>
    <constructor-arg name="name" value="红高粱"></constructor-arg>
</bean>
```

 设置属性值的方式：1.value 	2.ref （引用对象）

```xml
<bean id="boy" class="com.ohayoo.pojo.Boy">
    <property name="age" value="22"></property> 
    <property name="name" value="Tom"></property>
</bean>
<bean id="girl" class="com.ohayoo.pojo.Girl">
    <property name="age" value="22"></property>
    <property name="name" value="Lily"></property> //value赋值
    <property name="boyfriend" ref="boy"></property>  //ref对象引用
</bean>
```

#### 6.注解的引用

###### 什么是注解：

注解其实就是代码里的**特殊标记**，这些标记可以在编译、类加载、运行时被读取，并执行相应的处理。通过使用注解，程序员可以不改变原有逻辑的情况下，在源文件中嵌入一些补充信息。代码分析工具、开发工具和部署工具可以通过这些补充信息进行验证或者进行部署。

###### 注解的使用：

使用注解时要在其前面增加@符号，并把该注解当成一个修饰符使用。用于修饰它支持的程序元素（包、类、构造器、方法、成员变量、参数、局部变量的声明）。

###### 注解的重要性：

在JavaSE中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。在JavaEE/Arldroid中注解占据了更重要的角色，例如用来配置应用程序的任何切面，代替JavaEE旧版中所遗留的繁冗代码和XML配置等。未来的开发模式都是基于注解的。一定程度上可以说：框架=注解+反射+设计模式。

| 注解名称       | 解释                                                         |
| -------------- | ------------------------------------------------------------ |
| @Component     | 实例化Bean，默认名称为类名的字母变小写。无需指定setter方法   |
| @Ropository    | 作用和@Component一样。用在持久层                             |
| @Service       | 作用和@Component一样。用在业务层                             |
| @Contorller    | 作用和@Component一样。用在控制器层                           |
| @Configuration | 作用和@Component一样。用在配置类上                           |
| @Autowired     | 自动注入。默认byType，如果多个同类型bean，使用byName<br />添加@Autowired注解后会把容器中的对象自动注入进来，并且不需要依赖set方法 |
| @Value         | 给普通数据类型属性赋值，普通数据类型包括：八种基本数据类型+String<br />，并且不需要依赖set方法 |

 使用@Component，

```java
package com.ohayoo.pojo;

import org.springframework.stereotype.Component;

@Component
public class Girl {
    @Value("18")     //@Value标签给普通数据类型属性赋值
    private int age;
    @Value("Lily")    //@Value标签给普通数据类型属性赋值
    private String name;
		@Autowired       //@Autowired 会把容器中的对象自动注入进来
    private Boy boyfriend;
    ...
}
```

配置xml中加入包扫描

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" <!-- 1 -->
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
       	http://www.springframework.org/schema/context  <!-- 2 -->
        https://www.springframework.org/schema/context/spring-context.xsd">  <!-- 3 -->

   		 <context:component-scan base-package="com.ohayoo.pojo"></context:component-scan> <!-- 4 包扫描 -->
    
</beans>
```



**==================================================================================================================================================================================================================**



## SpringMVC环境搭建

#### 1.创建Maven-web项目

#### 2.补全目录

#### 3.在pom.xml中添加依赖（springmvc的依赖）

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

  <modelVersion>4.0.0</modelVersion>
  <groupId>com.ohayoo</groupId>
  <artifactId>TestSrpingMVC</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.16</version>
    </dependency>

  </dependencies>

</project>
```

#### 4.配置Tomcat容器，把web项目导入Tomcat容器中（不要使用版本10）

#### 5.创建控制器类，跳转到index.jsp

###### 1.控制器类

```java
package com.ohayoo.controller;

import com.ohayoo.pojo.Person;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class TestController {//控制器类
    @RequestMapping("/test1")
    public String test1(){
        //响应给浏览器index.jsp页面
        return "index.jsp";
    }
    @RequestMapping("test2")
    public String test2(Person p){
        System.out.println(p.getName()+"-----"+p.getAge());
        return "hello.jsp";
    }
}
```

###### 2.springmvc配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mv="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd 
        http://www.springframework.org/schema/mvc 
        https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!-- 扫描控制器类，千万不要把service等扫描进来，也千万不要在Spring配置文件中扫描配置文件扫描控制器类所在的包  -->
    <context:component-scan base-package="com.ohayoo.controller"></context:component-scan>
    <!-- 让Spring MVC的注解生效：@RequestMapping -->
    <mv:annotation-driven></mv:annotation-driven>

</beans>
```

###### 3.配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!-- 注册DispatcherServlet -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <!-- 关联springmvc配置文件 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
           <!-- springmvc.xml 名称自定义，只要和我们创建的配置文件的名称对应就可以了。     -->
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        
        <!-- Tomcat启动立即这行Servlet，而不是等到访问Servblet才去实力化DispatcherServlet   -->
        <!-- 配置上的效果：Tomcat启动立即加载Spring MVC框架的配置文件   -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!-- /表示除了.jsp结尾的uri，其他的uri都会触发DispatcherServlet。此处千万不要写成 /*        -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```



**==================================================================================================================================================================================================================**



## SSM的整合

#### 1.创建maven-war项目，删除pom.xml中多余的内容，留下

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  
  <groupId>com.ohayoo</groupId>
  <artifactId>TestSSM</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  
</project>
```

#### 2.补全目录

![Screenshot 2023-07-24 at 16.42.42](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-24 at 16.42.42.png)

#### 3.添加依赖和插件

1.mybatis的依赖

2.连接mysql的依赖

3.log4j的依赖

4.spring的核心依赖

5.springjdbc的依赖

6.spring整合mybatis的依赖

7.springwebmvc的依赖

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.ohayoo</groupId>
    <artifactId>TestSSM</artifactId>
    <packaging>war</packaging>
    <version>1.0-SNAPSHOT</version>

​    <dependencies>
​        <!--1.mybatis的依赖    -->
​        <dependency>
​            <groupId>org.mybatis</groupId>
​            <artifactId>mybatis</artifactId>
​            <version>3.5.6</version>
​        </dependency>
​        <!--2.连接mysql的依赖    -->
​        <dependency>
​            <groupId>mysql</groupId>
​            <artifactId>mysql-connector-java</artifactId>
​            <version>8.0.28</version>
​        </dependency>
​        <!--3.log4j    -->
​        <dependency>
​            <groupId>log4j</groupId>
​            <artifactId>log4j</artifactId>
​            <version>1.2.17</version>
​        </dependency>
​        <!--4.spring的核心依赖    -->
​        <dependency>
​            <groupId>org.springframework</groupId>
​            <artifactId>spring-context</artifactId>
​            <version>5.3.16</version>
​        </dependency>
​        <!--5.springjdbc依赖    -->
​        <dependency>
​            <groupId>org.springframework</groupId>
​            <artifactId>spring-jdbc</artifactId>
​            <version>5.3.16</version>
​        </dependency>
​        <!--6.spring整合mybatis的依赖    -->
​        <dependency>
​            <groupId>org.mybatis</groupId>
​            <artifactId>mybatis</artifactId>
​            <version>3.5.6</version>
​        </dependency>
​        <!--7.springwebmvc的依赖    -->
​        <dependency>
​            <groupId>org.springframework</groupId>
​            <artifactId>spring-webmvc</artifactId>
​            <version>5.3.16</version>
​        </dependency>

​    </dependencies>

</project>
```

#### 4.处理spring整合mybatis部分

整合前：数据库的配置有mybatis.xml来管理

整合后：mybatis.xml 要发生翻天覆天的变化了，都交由spring来管理，创建applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

​    <!--1.连接数据库，获取数据源，配置数据源，设置数据库连接的四个参数   -->
​    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
​        <!--利用setter方法完成属性注入，四个参数名固定的，注意源码中虽然没有driverClassName属性，但是也有driverClassName的setter的方法  -->
​        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
​        <property name="url" value="jdbc:mysql://127.0.0.1:3306/mygo"/>
​        <property name="username" value="root"/>
​        <property name="password" value="root"/>
​    </bean>

​    <!--2.获取SqlSessionFactory对象  -->
​    <!--以前SqlSessionFactory都是在测试代码中我们自己创建的，但是现在不用了，整合包中提供的对于SqlSessionFactory的封装。
​    里面提供了Mybatis全局配置文件所有配置的属性    -->
​    <bean id="factory" class="org.mybatis.spring.SqlSessionFactoryBean">
​        <!--注入数据源 -->
​        <property name="dataSource" ref="dataSource"/>
​        <!--给包下类起别名 -->
​        <property name="typeAliasesPackage" value="com.ohayoo.pojo"/>
​    </bean>

​    <!--3.扫描mapper文件  -->
​    <!--设置扫描哪个包，进行接口绑定  -->
​    <!--所有Mapper接口代理对象都能创建出来，可以直接从容器中获取出来  -->
​    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
​        <!-- 和SqlSessionFactory产生联系，以前接口绑定sqlSession.getMapper(BookMapper.class):
​          都是通过以前接口绑定sqlSession来调用mapper，所以这里一定要注入工厂
​          注意这里sqlSessionFactoryBeanName类型为String，所以用value把工厂名字写过来就行-->
​        <property name="sqlSessionFactoryBeanName" value="factory"/>
​        <!--扫描的包 -->
​        <property name="basePackage" value="com.ohayoo.mapper"/>
​    </bean>

​    <!--4.扫描包下注解    -->
​    <context:component-scan base-package="com.ohayoo.service"></context:component-scan>

</beans>
```

mybatis.xml需要配置有log4j

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <settings>
      <setting name="logImpl" value="LOG4J"/>
  </settings>

</configuration>
```

配置log4j.properties

```properties
log4j.rootLogger = ERROR,console
log4j.logger.com.ohayoo.mapper.BookMapper = TRACE

### console ###
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern = [%p] [%-d{yyyy-MM-dd HH:mm:ss}] %C.%M(%L) | %m%n


```

#### 5.整合springmvc

加入springmvc.xml的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!-- 扫描控制器类，千万不要把service等扫描进来，也千万不要在Spring配置文件中扫描配置文件扫描控制器类所在的包  -->
    <context:component-scan base-package="com.ohayoo.controller"></context:component-scan>
    <!-- 让Spring MVC的注解生效：@RequestMapping -->
    <mvc:annotation-driven></mvc:annotation-driven>

</beans>
```

#### 6.项目分层

###### 数据连接层（DAO层）： 

实体类  + 对象操作接口（抽象方法） + 对象类的Mapper.xml（抽象方法的实现）

文件路径保持一致：BookMapper.java.   BookMapper.xml

实体类：

```java
package com.ohayoo.pojo;

public class Book {
    private int id;
    private String name;
    private String author;
    private double price;

​	...

}
```

抽象接口：

```java
package com.ohayoo.mapper;

import java.util.List;

public interface BookMapper {
    List selectAll();
}
```

实现类：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ohayoo.mapper.BookMapper">
    <select id="selectAll" resultType="book">
        select * from t_book
    </select>
</mapper>
```



###### 业务逻辑层（Service层）：

接口：

```java
package com.ohayoo.service;


import java.util.List;

public interface BookService {
    List findAll();
}
```

service实现类：

```java
package com.ohayoo.service.impl;

import com.ohayoo.mapper.BookMapper;
import com.ohayoo.service.BookService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookMapper bookMapper;
    @Override
    public List findAll() {
        return bookMapper.selectAll();
    }
}
```



###### 控制层（Controller层 或 API层）：

```java
package com.ohayoo.controller;

import com.ohayoo.pojo.Book;
import com.ohayoo.service.BookService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.List;

@Controller
public class BookController {
    @Autowired
    private BookService bookService;

    @RequestMapping(value = "/findAllBooks", produces = "text/html;charset=utf-8")
    @ResponseBody
    public String findAll() {

        List list = bookService.findAll();
        System.out.println(list.size());
        String books = "";

        for (Object o : list) {
            Book book = (Book) o;
            books = books + book.getName();
        }

        return books;
    }
}

```



**==================================================================================================================================================================================================================**



## SpringBoot环境搭建

#### 1.创建Maven工程

普通maven工程

#### 2.选择springboot的版本（pom.xml配置）

```xml
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.7.6</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

#### 3.整合springmvc用到的包，添加启动器

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.7.6</version>
        </dependency>
    </dependencies>
```

#### 4.写一个Controller

#### 5.启动类编写

###### 扫描同包和子包的注解

###### 启动项目，只需要在启动类里运行

```java
package com.ohayoo;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication //启动类的注解
@MapperScan("com.ohayoo.mapper") //扫描Mapper
public class TestSpringBootApplication {
    public static void main(String[] args) {
        SpringApplication.run(TestSpringBootApplication.class,args);
    }
}

```

#### 6.resources目录下配置application.properties

#### 7.springboot整合SSM

######   1.导入（mybatis）依赖

```xml
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.3</version>
        </dependency>       
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.21</version>
        </dependency>
```

######   2.编写配置文件（DB连接配置） --resources/application.yml

```
server:
  port: 9999
  servlet:
    context-path: /springbootssm
spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/mygo
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: root
mybatis:
  type-aliases-package: com.ohayoo.pojo
  mapper-locations: classpath:mybatis/*.xml  //mapper路径

```

#### 8.项目分层

 Controller+service+dao
