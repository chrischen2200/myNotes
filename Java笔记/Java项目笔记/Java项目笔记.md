##### Java项目笔记

## 1.项目概述和环境搭建

### 1-1软件开发整体介绍

- #### 软件开发流程

  ##### 1）需求分析（需求规格说明书，产品原型） 

  ##### 2）设计（UI设计，数据库设计，接口设计）

  ##### 3）编码（项目代码，单元测试）

  ##### 4）测试（测试用例，测试报告）

  ##### 5）上线运维

- #### 角色分工

  ##### 1）项目经理：对整个项目负责，任务分配，把控进度

  ##### 2）产品经理：进行需求调研，输出需求调研文档，产品原型等

  ##### 3）UI设计师：根据产品原型输出界面效果图

  ##### 4）架构师：项目整体架构设计，技术选型等

  ##### 5）开发工程师：代码实现

  ##### 6）测试工程师：编写测试用例，输出测试报告

  ##### 7）运维工程师：软件环境搭建，项目上线

- #### 软件环境

  ##### 1）开发环境：开发人员在开发阶段使用的环境，一般外部用户无法访问

  ##### 2）测试环境：专门给测试人员使用的环境，用于测试项目，一般外部用户无法访问

  ##### 3）生产环境：即上线环境，正式提供对外服务的环境

### 1-2苍穹外卖项目介绍

- #### 项目介绍

  ##### 1）定位：专门为餐饮企业（餐厅，饭店）定的一款软件产品

  ##### 2）功能架构：体现项目中的业务功能模块

  ![Screenshot 2023-07-28 at 17.31.12](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-28 at 17.31.12.png)

- #### 产品原型

  ##### 1）产品原型：用于展示项目的业务功能，一般由产品经理进行设计（画面レイアウト，业务流程）

- #### 技术选型

  ##### 1）技术选型：展示项目中使用到的技术框架和中间件等

  ![Screenshot 2023-07-28 at 17.39.44](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-28 at 17.39.44.png)

### 1-3开发环境搭建

**整体结构**		![Screenshot 2023-07-28 at 17.43.01](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-28 at 17.43.01.png)

- #### 前端环境搭建

  ##### 前端工程基于*nignx*运行（

  ##### 1）nginx在https://nginx.org/en/download.html下载

  ##### 2）启动nginx（start nginx/sudo nginx）---重启（nginx -s reload），关闭（nginx -s stop/sudo nginx -s stop）

  ##### 3）把项目放在html目录下

  ##### 4）修改confg目录下nginx.conf的配置文件

  ##### 5）启动web项目，路径：http://localhost:端口![Screenshot 2023-07-28 at 17.45.59](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-28 at 17.45.59.png)

- #### 后端环境搭建

  ##### 1）后端工程基于*maven*进行项目构建，并且进行分模块开发	![Screenshot 2023-07-28 at 22.46.13](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-28 at 22.46.13.png)

  ##### 2）用IDEA打开初始工程，了解项目的整体结构

  ![Screenshot 2023-07-28 at 22.48.45](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-28 at 22.48.45.png)		

  ![Screenshot 2023-07-28 at 22.49.17](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-28 at 22.49.17.png)		![Screenshot 2023-07-28 at 22.51.38](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-28 at 22.51.38.png)

  ![Screenshot 2023-07-28 at 22.53.12](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-28 at 22.53.12.png)

  ![Screenshot 2023-07-28 at 22.56.05](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-28 at 22.56.05.png)

  ##### 3）使用Git进行项目代码控制，具体操作

  ###### 		a.创建Git本地仓库

  ###### 		b.创建Git远程仓库

  ###### 		c.将本地文件推送到Git远程仓库

  ##### 4)数据库环境搭建

  ​	通过数据库建表语句创建数据库表结构

  ​		![Screenshot 2023-07-28 at 23.56.38](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-28 at 23.56.38.png)

  ##### 5）前后端链联调

  ![Screenshot 2023-07-29 at 0.04.32](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 0.04.32.png)		

  注：可以通过断点调试跟踪后端程序的执行过程

  注：前端发送请求，是如何请求到后端服务的呢？

  ​		nginx反向代理，就是将前端发送的动态请求由nginx转发到后端服务器

  ​		nginx反向代理的好处：

  ​			提高访问速度			

  ​			进行负载均衡：所谓负载均衡，就是把大量的请求按照我们指定的方式均衡的分配给集群中的每台服务器

  ​			保证后端服务安全		![Screenshot 2023-07-29 at 15.48.27](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 15.48.27.png)

  ![Screenshot 2023-07-29 at 15.50.21](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 15.50.21.png)

  ![Screenshot 2023-07-29 at 15.54.00 1](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 15.54.00 1.png)

- #### 完善登陆功能

**问题：员工表中的密码是明文存储，不安全**

**思路：1.将密码加密后存储，提高安全性。2.使用MD5方式对明文密码加密**（MD5不可逆）

1）修改数据库中明文密码，改为MD5加密后的密文

2）修改Java代码，前端提交的密码进行MD5加密后再跟数据库中密码比对

spring提供的方法：

![Screenshot 2023-07-29 at 16.03.09](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 16.03.09.png)

### 1-4导入接口文档

- #### 前后端分离开发流程

  ![Screenshot 2023-07-29 at 16.33.06](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 16.33.06.png)

- #### 操作步骤![Screenshot 2023-07-29 at 16.34.18](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 16.34.18.png)

### Swagger

![Screenshot 2023-07-29 at 16.45.20](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 16.45.20.png)

![Screenshot 2023-07-29 at 16.51.33](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 17.03.45.png)

## 2.员工原理、分类管理

#### 2-1新增员工

- ##### 需求分析和设计

  1）产品原型：账号（必须唯一），手机号（11位），身份号码（18位），默认密码（123456）

  2）接口设计：	![Screenshot 2023-07-29 at 17.18.54](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 17.18.54.png)

  3）数据库设计：

  ![Screenshot 2023-07-29 at 17.20.52](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 17.20.52.png)

- ##### 代码开发

- ##### 功能测试

  功能测试方式：

  1）通过接口文档测试

  2）通过前后端联调测试

- ##### 代码完善

  程序存在的问题：

  1）录入的用户名已存在，跑出异常后没有处理

  重写全局异常处理方法

  ![Screenshot 2023-07-29 at 20.27.55](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 20.27.55.png)

  2）新增员工时，创建id和修改人id设置为了固定值

  拦截器里解析修改人id，通过ThreadLocal把修改人id值传给service

  注：每一次请求都是一个线程

###### ![Screenshot 2023-07-29 at 20.58.54](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 20.58.54.png)

![Screenshot 2023-07-29 at 21.02.13](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 21.02.13.png)

#### 2-2员工分页查询

![Screenshot 2023-07-29 at 21.46.22](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 21.46.22.png)

 时间显示问题的解决方式：

![Screenshot 2023-07-29 at 22.18.26](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 22.18.26.png)

![Screenshot 2023-07-29 at 22.18.26](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-29 at 22.19.05.png)



#### 2-3启用禁用员工账号

#### 2-4编辑员工

#### 2-5导入分类模块功能代码

## 3.公共字段自动填充（AOP，自定义注解）

 		**问题：公共字段重复操作，代码冗余**

![Screenshot 2023-07-30 at 17.13.59](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-30 at 17.13.59.png)

![Screenshot 2023-07-30 at 17.13.40](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-30 at 17.13.40.png)

## 4.菜品删除

 **当操作表时，在serveice实现类的方法上加事务注解@Transactional**

## 5.Redis缓存（框架：Spring Data）

## 6.Spring Task
