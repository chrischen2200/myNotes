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

  jpa:
    show-sql: true
    hibernate:
      ddl-auto: update
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

如何访问数据的表，**需要自定义接口继承接口JpaRepository**

DAO接口:

```java
package com.example.demo.dao;

import com.example.demo.pojo.Book;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface BookRepository extends JpaRepository<Book,Integer> {
    List<Book> findAllByName(String name); //自己带的方法直接调用即可，其他的方法按一定规则自定义（SQL）
}

```

#### 5.使用JPQL的方式查询

使用Spring Data JPA提供的查询方法已经可以解决大部分的应用场景了，但是对于某些业务来说，我们还需要灵活的构造查询条件，这时就可以使用@Query注解，结合JPQL的语句方式完成查询。

###### @Query 注解的是用非常简单，只需要在方法上标注该注解，同时提供一个JPQL查询语句即可，注意：

- ###### 大多数情况下将*替换为别名

- ###### 表名改为类名

- ###### 字段改为属性名

- ###### 搭配注解@Query进行使用

  ```java
  		//1.入门    
  		@Query("select b from Book b")//从实体类，查询，而不是表
      List<Book> findAllBook();
  
  		//2.筛选条件
      @Query("select b from Book b where b.name='西游记'") //从实体类，查询，而不是表，where不是列名，而不是属性名
      List<Book> findAllBookByName();
  
  		//3.投影结果
   		@Query("select b.name from Book b") //真实的案例中，我们不需要全部的数据，只需要1列数据
      List<String> findAllBook2();  //类型根据所查询的数据所决定
  
  		//4.聚合查询
  		@Query("select count(b) from Book b")
      int findCount();
  		
  		//5.传参
      //修改数据 一定加上@Modifying 注解
      @Transactional //标记事务，可以用与service层和测试类
      @Modifying
      @Query("update Book set author=?1 where id=?2")
      int updateAuthorById(String Author, Integer id);
  
  		//6.原生sql，对表操作
      @Transactional
      @Modifying
      @Query(value="update t_book set author=?1 where id=?2",nativeQuery = true)
      int updateAuthorById2(String Author, Integer id);
  		
  ```



#### 6.一对一关联查询

###### 	1.创建实体类：

```java
package com.example.demo.pojo;

import jakarta.persistence.*;
import lombok.Data;

@Data
@Entity
@Table(name="account")
public class Account {
    
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name="id")
    @Id
    int id;
    @Column(name="username")
    String username;
    @Column(name="password")
    String password;
    
    @JoinColumn(name="detail_id") //外键
    @OneToOne(fetch = FetchType.LAZY,cascade = CascadeType.ALL) //设置1对1。 关联操作为ALL
    AccountDetail detail;  //关联实体类（表）
}
```

```java
package com.example.demo.pojo;

import jakarta.persistence.*;
import lombok.Data;

@Data
@Entity
@Table(name = "account_detail")
public class AccountDetail {

    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    @Id
    int id;
    @Column(name = "address")
    String address;
    @Column(name = "email")
    String email;
    @Column(name = "phone")
    String phone;
    @Column(name = "real_name")
    String realName;
}

```

###### 	2.DAO

```java
package com.example.demo.dao;

import com.example.demo.pojo.Account;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface AccountRepository extends JpaRepository<Account,Integer> {
}
```

```java
package com.example.demo.dao;

import com.example.demo.pojo.AccountDetail;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface AccountDetailRepository extends JpaRepository<AccountDetail,Integer> {
}
```

###### 	3.增删查改（service）

```java
	@Autowired
	AccountRepository accountRepository;
	@Autowired
	AccountDetailRepository accountDetailRepository;

	@Test
	public void testOneToOne(){
		Account account = new Account();
		account.setUsername("chris");
		account.setPassword("1234567");

		AccountDetail accountDetail = new AccountDetail();
		accountDetail.setPhone("07011112222");
		accountDetail.setRealName("Chen");
		accountDetail.setAddress("Yokohama");
		accountDetail.setEmail("masa@gmail.com");

		account.setDetail(accountDetail);

		//增
		System.out.println("新增数据的ID："+accountRepository.save(account));
		//删
		accountRepository.deleteById(1);

	}
```

#### 7.一对多

###### 1.创建实体类

```java
package com.example.demo.pojo;

import jakarta.persistence.*;
import lombok.Data;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.List;

@Data
@Entity
@Table(name="author")
public class Author  {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name="id")
    private Long id;

    @Column(name = "name",nullable = false,length = 20)
    private String name;

    //级联保存，更新，删除，刷新；延迟加载，党删除用户，会级联删除该用户的所用文章
    //用户mappedBy注解的实体类为关系被维护端
    //mappedBy=”author“中的author的是Article的author属性
    @OneToMany(mappedBy = "author",cascade = CascadeType.ALL,fetch = FetchType.EAGER)
    private List<Article> articleList;

}
```

```java
package com.example.demo.pojo;

import jakarta.persistence.*;
import lombok.Data;
import lombok.ToString;


@Data
@ToString(exclude = {"author"})
@Entity
@Table(name="article")
public class Article {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) //自增长策略
    private Long id;

    @Column(name="title", nullable = false,length = 50)//映射为字段，但是不能为空
    private String title;

    @Lob //大对象，映射，MySql 的Long Text 类型
    @Basic(fetch = FetchType.LAZY) //懒加载
    @Column(name ="content",nullable = false) //映射为字段，但是不能为空
    private String content;

    @ManyToOne(cascade = {CascadeType.MERGE,CascadeType.REFRESH},optional=false)//可选属性optional=false，表示author不能为空，删除文章，不影响用户
    @JoinColumn(name="author_id")//设置article表中的关联字段（外键盘）
    private Author author;
}

```



###### 2.DAO

```java
package com.example.demo.dao;

import com.example.demo.pojo.Author;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface AuthorRepository extends JpaRepository<Author,Long> {
}
```

```
package com.example.demo.dao;

import com.example.demo.pojo.Article;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ArticleRepository extends JpaRepository<Article,Long> {
}
```



###### 3.增删改查

```java
    @Autowired
    AuthorRepository authorRepository;
    @Autowired
    ArticleRepository articleRepository;

    @Test
    public void testOneToMany1() {
        Author author = new Author();
        author.setName("chris");
        Author author1 = authorRepository.save(author);

        Article article1 = new Article();
        article1.setTitle("112");
        article1.setContent("事实是谁时");
        article1.setAuthor(author1);
        articleRepository.save(article1);

        Article article2 = new Article();
        article2.setTitle("3");
        article2.setContent("43242");
        article2.setAuthor(author1);
        articleRepository.save(article2);

        authorRepository.findById(1L).ifPresent(System.out::println);


    }
```

#### 8.多对多

###### 1.创建实体类

```java
package com.example.demo.pojo;

import jakarta.persistence.*;
import lombok.Data;

import java.util.List;

@Data
@Entity
@Table(name= "user")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false,length = 20,unique = true)
    private String username;

    @Column(length = 100)
    private String password;

    @ManyToMany
    @JoinTable(name="user_authority",joinColumns = @JoinColumn(name ="user_id"),
    inverseJoinColumns = @JoinColumn(name ="authority_id"))
    private List<Authority> authorityList;

}
```

```java
package com.example.demo.pojo;

import jakarta.persistence.*;
import lombok.Data;

import java.util.List;

@Data
@Entity
@Table(name= "authority")
public class Authority {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    @Column(nullable = false)
    private String name;

    @ManyToMany(mappedBy = "authorityList")
    private List<User> userList;

}
```



###### 2.DAO

```java
package com.example.demo.dao;

import com.example.demo.pojo.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository  extends JpaRepository<User,Long> {
}

```

```java
package com.example.demo.dao;

import com.example.demo.pojo.Authority;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface AuthorityReposity extends JpaRepository<Authority,Integer> {
}
```

###### 3.增删改查

```java
    @Autowired
    UserRepository userRepository;
    @Autowired
    AuthorityReposity authorityReposity;
    @Test
    public void testManyToMany(){
        
        Authority authority1 = new Authority();
        authority1.setName("查看");
        List<User> list =new ArrayList<>() ;
        authority1.setUserList(list);
        authorityReposity.save(authority1);
        
        User user2 = new User();
        user2.setUsername("lisa");
        user2.setPassword("242342");
        Authority authority = authorityReposity.findById(1).get();
        List<Authority> list2 = new ArrayList<>();
        list2.add(authority);
        user2.setAuthorityList(list2);
        userRepository.save(user2);
    }
```

