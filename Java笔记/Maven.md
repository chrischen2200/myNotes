## Maven学习笔记

#### Maven项目目录结构

| src         |      |           |             |           |
| ----------- | ---- | --------- | ----------- | --------- |
|             | main |           |             |           |
|             |      | Java      |             |           |
|             |      |           | com.aaa.bbb |           |
|             |      |           |             | ontroller |
|             |      |           |             | servicec  |
|             |      |           |             | dao       |
|             |      | resources |             |           |
|             | test |           |             |           |
|             |      | Java      |             |           |
|             |      |           | com.aaa.bbb |           |
|             |      | resources |             |           |
| **pom.xml** |      |           |             |           |



#### Maven项目构建命令

- ###### Maven构建命令使用mvn开头，后面添加功能参数，可以一次执行过个命令，使用空格分隔

- ###### 在pom.xml文件所在的目录层级下执行

  | 命令        | 说明                                                         |
  | ----------- | ------------------------------------------------------------ |
  | mvn compile | #编译 下载包到本地仓库，并且生成target目录                   |
  | mv clean    | #清理 清理编译之后的target目录                               |
  | mvn test    | #测试 下载测试的插件 ，并且在target目录下生成测试结果文件    |
  | mvn package | #打包 编译，测试，打包，在target目录下生成，jar或war包       |
  | mvn install | #安装到本地仓库 把生成的包放到本地仓库（模块化开发的时候，把别人开发的模块变成包） |

  

#### Maven依赖范围

![Screenshot 2023-07-26 at 20.49.19](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-26 at 20.49.19.png)

#### Maven生命周期

#### Maven项目聚合

![Screenshot 2023-07-26 at 21.40.39](/Users/chris/Desktop/MyImgs/Screenshot 2023-07-26 at 21.40.39.png)

######             模块依赖关系维护

​             

