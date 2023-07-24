## Mybatis项目（接口绑定方案）

#### 1.创建新的Maven项目

#### 2.配置pom.xml（各项依赖包的坐标，包含mybatis）

#### 3.配置全局配置文件（/resources/mybatis.xml）

​	对象类别名：

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
<mapper namespace="com.ohayoo.mapper.BookMapper"> //全限定路径
    <select id="selectAllBooks" resultType="Book"> //id：接口的方法名  resultType：类的别名
        select * from t_book
    </select>
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

配置xml中加入包扫描<context:component-scan base-package="com.ohayoo.pojo"></context:component-scan>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" <!-- 1 -->
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
       	http://www.springframework.org/schema/context  <!-- 2 -->
        https://www.springframework.org/schema/context/spring-context.xsd">  <!-- 3 -->

   		 <context:component-scan base-package="com.ohayoo.pojo"></context:component-scan> <!-- 4 -->
    
</beans>
```



**==================================================================================================================================================================================================================**



## Web项目构建（war项目）

#### 1.创建Maven项目

#### 2.配置Tomcat容器，把web项目导入Tomcat容器中
