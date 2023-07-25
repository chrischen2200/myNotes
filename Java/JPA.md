# 使用JPA持久化对象的步骤

springboot框架基础上

#### 1.配置数据源

###### application.yml

```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mygo
    username: root
    password: root
```

#### 2.导入依赖（pom.xml）

```xml
//jpa		
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>

//对应数据库的驱动
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>8.0.21</version>
		</dependency>
```

#### 3.创建实体类

Java中实体类和数据库中表对应：属性 对应 字段

```java
package com.example.demo.pojo;

import jakarta.persistence.*;
import lombok.Data;

@Data //setter，getter等
@Entity //这是实体类
@Table(name = "t-book") //对应数据库哪张表
public class Book {
    @Id //这是主键
    @Column(name = "id") //表中的id字段，对应实体类中的id属性
    @GeneratedValue(strategy = GenerationType.IDENTITY) //主键值
    int id;

    @Column(name = "name") //实体类和表对应
    String name;

    @Column(name = "author") //实体类和表对应
    String author;
    
    @Column(name = "price") //实体类和表对应
    double price;
}
```

#### 4.基本的增删改查

如何访问数据的表，需要自定义接口继承接口JpaRepository

Dao:

```java
package com.example.demo.dao;

import com.example.demo.pojo.Book;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface BookRepository extends JpaRepository<Book,Integer> {
    List<Book> findAllByName(String name); //自定义方法（SQL）
}

```

自定义方法有一定的规则

#### 5.使用JPQL的方式查询

