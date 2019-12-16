# **Maven项目管理工具**

# 一、**Maven概述[了解]**

### 1．**问题引入**

 ![1576483068494](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576483068494.png)

l 目前存在的问题

运用我们目前学习的技术已经可以开发一个小型的项目了，但是在实际开发中，我们的项目规模要复杂的多，遇到的问题也更多！比如：

1、jar包的管理：多个项目依赖同一个jar包，要复制多次，jar升级时又得重新复制多次，jar之间还可能有多重依赖关系，容易管理混乱

2、项目的管理：项目规模越来越大，需要拆分成多个子模块，模块之间的相互依赖关系需要统一管理，并且项目生命周期中的编译，打包，测试，运行等步骤都需要统一管理

l 如何解决？

 ![1576483131060](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576483131060.png)

开发一个工具对jar包和项目进行统一的管理，比如：

把jar包都编个坐标，记录并存放在一个地方（这个地方称作为仓库），项目中要用哪个jar就根据坐标来仓库中找就行了；

对项目生命周期和模块进行统一管理，能够自动化的执行编译，打包，测试，运行等操作。

而我们想到的这些解决方案，早就有大牛帮我们实现好了，那就是Maven！

总结：通俗的说:maven就是用来管理jar包+管理项目

注意：这些工具都是帮助/辅助我们工作的，我们最终的开发产出物都是代码

### 2．**初识Maven**

l 官网

<http://maven.apache.org/>

l 百科介绍

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps3.jpg) 

l 说人话

Maven是一个项目管理工具，可以对项目和jar包进行统一个管理，包括：项目的构建（执行项目的生命周期）、项目的生命周期（编译、测试、打包、部署等）、项目的模块依赖关系和jar包的依赖关系

l Jar包管理

l 自动化的项目构建

### 3．**Maven的相关概念**

#### （1）**项目对象模型（POM）**

 	Project Object Model：POM对象模型，其实就是一个xml文件，名字叫做pom.xml，每个Maven工程中都有一个pom.xml文件，定义工程（所依赖的jar包）、（本工程的坐标、打包（jar/war）运行方式）。

Maven通过坐标对项目工程所依赖的jar包统一规范管理

企业使用时，也叫做GAV坐标

Maven的坐标使用如下三个量在 Maven 的仓库中唯一的确定一个jar。

[1] groupid：公司或组织的域名倒序+[当前项目名称] 

[2] artifactId：当前项目的模块名称

[3] version：当前模块的版本

例如：要引入junit的测试jar，只需要在pom.xml配置文件中配置引入junit的坐标即可

```java
<dependency>

<groupId>junit</groupId>

<artifactId>junit</artifactId>

<version>4.12</version>

<scope>test</scope>

</dependency>
```

#### （2）**生命周期**

l 清理、编译、测试、报告 、打包、部署、站点生成。

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps4.png) 

①清理：删除以前的编译结果，为重新编译做好准备。

②编译：将Java 源程序编译为字节码文件。

③测试：针对项目中的关键点进行测试，确保项目在迭代开发过程中关键点的正确性。

④报告：在每一次测试后以标准的格式记录和展示测试结果。

⑤打包：将一个包含诸多文件的工程封装为一个压缩文件用于安装或部署。Java 工程对应 jar 包，Web工程对应 war 包。

⑥安装：在 Maven 环境下特指将打包的结果——jar 包安装到本地仓库中或把 war包安装到web容器中。

⑦部署：将打包的结果部署到远程仓库或将war包部署到服务器上运行。

#### （3）**Maven项目标准目录结构**

Maven是约定思想的体现，约定>配置>编程，maven之前有一个ant工具（告诉它你的源代码在哪个路径下，然后编译输出到哪个路径）

l Maven工程有自己标准的目录结构。

而 Maven 正是因为指定了特定目录保存文件才能够对我们的 Java 工程进行自动化构建(就是自动执行上面的生命周期)。

l 标准目录结构示例

Project

  |-src

  |   |-main

  |   |  |-java        		—— 存放项目的.java文件

  |   |  |-resources   		——存放项目资源文件，如spring, hibernate配置文件

​        |-webapp    		—— webapp目录是web工程的主目录

​            |-WEB-INF

​              |-web.xml

  |   |-test

  |      |-java        		—— 存放所有测试.java文件，如JUnit测试类

  |      |-resources   		—— 测试资源文件

  |-target             		——目标文件输出位置例如.class、.jar、.war文件

  |-pom.xml           		——maven项目核心配置文件

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps5.jpg) 

#### （4）**Maven插件**

maven 管理项目生命周期过程都是基于插件完成的，例如：开发中使用的tomcat插件。

#### （5）**Maven仓库**

 ![1576483374423](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576483374423.png)

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps7.jpg) 

| 仓库名称             | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| 本地仓库             | 相当于缓存，工程第一次会从远程仓库（互联网）去下载jar 包，将jar包存在本地仓库（在程序员的电脑上）。第二次不需要从远程仓库去下载。先从本地仓库找，如果找不到才会去远程仓库找。 |
| 中央仓库（远程仓库） | 是一种远程仓库，仓库中的jar包由专业团队（maven团队）统一维护。里面存放了全世界大多数流行开源软件jar包中央仓库的地址：<http://mvnrepository.com/tags/maven> |
| 私服（远程仓库）     | 在公司内部架设一台私服，其它公司架设一台仓库，对外公开。     |

 

### 4．**使用Maven的好处**

通过上边介绍传统项目和maven项目在项目构建及依赖管理方面的区别，maven有如下的好处：

1、自动构建（生命周期管理）：maven对项目构建的过程进行标准化，通过一个命令即可完成构建过程。

2、依赖管理：maven工程不用手动导jar包，通过在pom.xml中定义坐标从maven仓库自动下载，方便且不易出错。

3、跨平台：maven命令可在window、linux上使用，命令无差别。

4、提升效率：遵循maven规范开发有利于提高大型团队的开发效率，降低项目的维护成本，大公司都会考虑使用maven来构建项目。

​	团队>个人

# 二、**Maven实战[应用]**

### 1．**安装配置**

l 下载

<http://maven.apache.org/download.cgi> 

l 安装

解压到指定软件安装目录即可

l 目录介绍

bin：存放执行脚本文件的地方

boot：存放一些扩展的地方

conf：maven的核心配置文件存放的路径

lib：maven的依赖包

l 配置环境变量

1、配置JAVA_HOME和MAVEN_HOME

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps8.jpg) 

2.配置path

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps9.jpg) 

l 验证

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps10.jpg) 

l 配置本地仓库

 ![1576483429511](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576483429511.png)

1.在maven的安装目录中conf/ settings.xml文件，在这里配置本地仓库

指定本地仓库位置(默认在 ${user.dir}/.m2/repository，${user.dir}表示windows用户目录)

​	<localRepository>C:\develop\repository</localRepository>

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps12.jpg) 

2.指定阿里云镜像位置（中央仓库默认不用配置，此处只是使用阿里云镜像替代中央仓库，提高jar的下载速度）

```java
<mirror>
<id>alimaven</id>
<name>aliyunmaven</name>
<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
<mirrorOf>central</mirrorOf>        
</mirror>
```

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps13.jpg) 

### 2．**Eclipse关联Maven**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps14.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps15.jpg) 

### 3．**IDEA关联Maven**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps16.jpg) 

### 4．**使用Maven管理项目(重点)**

l 目录规范

使用maven创建的工程我们称它为maven工程，maven工程具有一定的目录规范，如下：

src/main/java 					—— 存放项目的.java文件

src/main/resources 			—— 存放项目资源文件，如spring,hibernate配置文件

src/test/java					—— 存放所有单元测试.java文件，如JUnit测试类

src/test/resources 			—— 测试资源文件

target 							—— 项目输出位置，编译后的class文件会输出到此目录

pom.xml						——maven项目核心配置文件

l 项目示例

下面是一个完整的使用Maven管理的JavaWeb项目的目录结构

Project

  |-src

  |   |-main

  |   |  |-java        	—— 存放项目的.java文件

  |   |  |-resources   	—— 存放项目资源文件，如spring, hibernate配置文件

​         |-webapp     	—— webapp目录是web工程的主目录

​            |-WEB-INF

​              |-web.xml

  |   |-test

  |      |-java        	——存放所有测试.java文件，如JUnit测试类

  |      |-resources  	—— 测试资源文件

  |-target             	—— 目标文件输出位置例如.class、.jar、.war文件

  |-pom.xml          	——maven项目核心配置文件

 

**Maven骨架：其实就是maven创建不同的项目结构**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps17.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps18.jpg) 

#### （1）**普通Java项目**

##### ①**创建项目**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps19.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps20.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps21.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps22.jpg) 

##### ②**编写代码**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps23.jpg) 

##### ③**编写测试**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps24.jpg) 

l 添加Test依赖

可以通过<http://mvnrepository.com/tags/maven>网址搜索jar包的坐标

 ```xm
<dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
</dependencies>
 ```



配置maven的编译级别

```xm
<!--编译插件，可以配置编译版本，因为maven默认是按照1.5进行编译的-->
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
                <encoding>utf-8</encoding>
            </configuration>
        </plugin>
    </plugins>
</build>
```



测试用例（Use Case）：针对一个功能或者一个功能点的测试逻辑

 

 

#### （2）**JavaWeb项目**

##### ①**创建项目**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps25.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps26.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps27.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps28.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps29.jpg) 

l 解决IntelliJ IDEA创建Maven项目速度慢问题

<https://www.cnblogs.com/del88/p/6286887.html>

增加属性：archetypeCatalog=internal

或者

全局属性

**在maven的VM Options加上** **-DarchetypeCatalog=internal** **参数，如下：** 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps30.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps31.jpg) 

##### ②**完善目录**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps32.jpg) 

##### ③**添加依赖**

 ```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.bigdata</groupId>
    <artifactId>maven_02</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!-- Servlet -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <!--只需编译无需打包,tomcat自带-->
            <scope>provided</scope>
        </dependency>
        <!-- JSP -->
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
            <!--只需编译无需打包,tomcat自带-->
            <scope>provided</scope>
        </dependency>
        <!-- JSTL -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
            <!--只是运行时依赖-->
            <scope>runtime</scope>
        </dependency>
    </dependencies>

    <build>
         <plugins>
            <!--配置编译级别-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <!--打包跳过单元测试-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.18.1</version>
                <configuration>
                    <skipTests>true</skipTests>
                </configuration>
            </plugin>
            <!-- Tomcat -->
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                    <path>/${project.artifactId}</path>
                    <port>8080</port>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
 ```



##### ④**编写代码**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps33.jpg) 

 ```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Date;

@WebServlet("/hello")
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().write(new Date().toString());
    }
}
 ```



##### ⑤**部署运行**

l 添加到外置Tomcat运行

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps34.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps35.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps36.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps37.jpg) 

<http://localhost:8080/hello>

l 使用Tomcat插件运行

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps38.jpg) 

或手动指定命令

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps39.jpg) 

### 5．**常见Maven命令**

#### （1）**clean**

clean是maven工程的清理命令，执行 clean会删除整个target目录及内容。

#### （2）**compile**

compile是maven工程的编译命令，作用是将src/main/java下的文件编译为class文件输出到target目录下。

#### （3）**test**

test是maven工程的测试命令，会执行src/test/java下的单元测试类。

注意：

1、需要注掉pom.xml中打包跳过测试那一段

2、Maven要求测试类放在src/test/java目录下，且测试方法的

类名以Test结尾、方法名以test开头

#### （4）**package**

package是maven工程的打包命令，对于java工程执行package打成jar包，对于web工程打成war包。

#### （5）**install**

install是maven工程的安装命令，执行install将项目打成jar包或war包发布到本地仓库。

# 三、**Maven扩展(了解)**

### 1．**Maven的生命周期**

l 何为生命周期？

• Maven生命周期就是为了对所有的构建过程进行抽象和统一

• 包括项目清理，初始化，编译，打包，测试，部署等几乎所有构建步骤

l Maven三大生命周期

生命周期Maven有三套相互独立的生命周期，

• clean：在进行真正的构建之前进行一些清理工作

• default：构建的核心部分，编译，测试，打包，部署等等

• site：生成项目报告，站点，发布站点

它们是相互独立的，你可以仅仅调用clean来清理工作目录，仅仅调用site来生成站点。当然你也可以直接运行 mvn clean install site 运行所有这三套生命周期。

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps40.jpg) 

#### （1）**clean生命周期**

clean生命周期每套生命周期都由一组阶段(Phase)组成，我们平时在命令行输入的命令总会对应于一个特定的阶段。比如，运行mvn clean ，这个的clean是Clean生命周期的一个阶段。有Clean生命周期，也有clean阶段。Clean生命周期一共包含了三个阶段： 

l pre-clean 执行一些需要在clean之前完成的工作 

l clean 移除所有上一次构建生成的文件 

l post-clean 执行一些需要在clean之后立刻完成的工作 

mvn clean 中的clean就是上面的clean，在一个生命周期中，运行某个阶段的时候，它之前的所有阶段都会被运行，也就是说，mvn clean 等同于 mvn pre-clean clean ，如果我们运行 mvn post-clean ，那么 pre-clean，clean 都会被运行。这是Maven很重要的一个规则，可以大大简化命令行的输入。

#### （2）**site生命周期**

l pre-site 执行一些需要在生成站点文档之前完成的工作 

l site 生成项目的站点文档 

l post-site 执行一些需要在生成站点文档之后完成的工作，并且为部署做准备 

l site-deploy 将生成的站点文档部署到特定的服务器上 

这里经常用到的是site阶段和site-deploy阶段，用以生成和发布Maven站点，这可是Maven相当强大的功能，Manager比较喜欢，文档及统计数据自动生成，很好看。 

#### （3）**default生命周期**

l Default生命周期

Default生命周期是Maven生命周期中最重要的一个，绝大部分工作都发生在这个生命周期中。这里，只解释一些比较重要和常用的阶段： 

l validate 

l generate-sources 

l process-sources 

l generate-resources 

l process-resources 复制并处理资源文件，至目标目录，准备打包。 

l compile 编译项目的源代码。 

l process-classes 

l generate-test-sources 

l process-test-sources 

l generate-test-resources 

l process-test-resources 复制并处理资源文件，至目标测试目录。 

l test-compile 编译测试源代码。 

l process-test-classes 

l test 使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署。 

l prepare-package 

l package 接受编译好的代码，打包成可发布的格式，如 JAR 。 

l pre-integration-test 

l integration-test 

l post-integration-test 

l verify 

l install 将包安装至本地仓库，以让其它项目依赖。 

l deploy 将最终的包复制到远程的仓库，以让其它开发人员与项目共享。 

运行任何一个阶段的时候，它前面的所有阶段都会被运行

这也就是为什么我们运行mvn install 命令的时候，代码会被编译，测试，打包。

 

注意：default生命周期中不包含clean,开发中如果修改代码后打包运行没有更改的效果,需要重新执行clean命令

### 2．**Maven依赖范围**

依赖：项目需要依靠一个jar

依赖范围：依赖一个jar，并不一定是在项目整个生命周期都需要它，可能只是在某一个阶段需要，那么依赖范围就是在定义在哪些阶段依赖这个jar

A依赖B，需要在A的pom.xml文件中添加B的坐标，大家注意到我们之前添加坐标时还有写了一个scope ，这是依赖的范围。

scope有几个可选值， 如：

1、compile： 默认值，表示编译依赖范围。即编译、测试、运行时都需要，会被打包。（默认值compile表明该jar一直全程存在/需要）

2、test：表示测试依赖范围。即测试时需要，编译和运行时不需要，不会被打包。比如：junit。

3、provided：表示已提供依赖范围。即编译、测试时需要，运行时不需要，不会被打包。比如：servlet-api和jsp-api被tomcat容器提供。

服务器本身会提供这些jar，避免和服务器上的这些包冲突

4、runtime:表示运行时提供依赖范围。即编译时不需要，运行和测试时需要，会被打包。比如：jstl、jdbc驱动。

5、system：system范围依赖与provided类似，但是你必须显式的提供一个对于本地系统中JAR文件的路径，需要指定systemPath磁盘路径，system依赖不推荐使用。

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps41.jpg) 

l 测试各个scope

package打war观察jsp-api和servlet-api是否在war中存在？不在

package打war观察mysql-connctor-java是否在war中存在？在

 ```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
<!-- Servlet -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <!--只需编译无需打包,tomcat自带-->
    <scope>provided</scope>
</dependency>
<!-- JSP -->
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.2</version>
    <!--只需编译无需打包,tomcat自带-->
    <scope>provided</scope>
</dependency>
<!-- JSTL -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
    <!--只是运行时依赖-->
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.40</version>
    <scope>runtime</scope>
</dependency>
 ```



l 测试总结：

ü 默认引入 的jar包				---compile 【默认范围 可以不写】（编译、测试、运行 都有效 ）

ü servlet-api 、jsp-api 		   ---provided （编译、测试 有效， 运行时无效 防止和tomcat下jar冲突）

ü jdbc驱动jar包 				---runtime （测试、运行 有效 ）

ü junit							---test （测试有效）

依赖范围由强到弱的顺序是：compile>provided>runtime>test

总结：

开发中一般使用默认compile即可,如果有错误再进行修改,修改的原则也很简单记住常用的几个provided:servlet和jsp

### 3．**Maven的插件(增强功能)**

l Maven的核心仅仅定义了抽象的生命周期，具体的任务都是交由插件完成的，每个插件都能实现多个功能，每个功能就是一个插件目标

l maven插件可以完成一些特定的功能。例如，集成jdk插件可以方便的修改项目的编译环境；集成tomcat插件后，无需安装tomcat服务器就可以运行tomcat进行项目的发布与测试。

l 在pom.xml中通过plugin标签引入maven的功能插件。

```xml
<plugins>
  <!-- Compile -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.3</version>
    <configuration>
      <source>1.8</source>
      <target>1.8</target>
      <encoding>UTF-8</encoding>
    </configuration>
  </plugin>
  <!-- 打包跳过Test -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>2.18.1</version>
    <configuration>
      <skipTests>true</skipTests>
    </configuration>
  </plugin>
  <!--   配置Tomcat插件 -->
  <plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
    <configuration>
      <!--<path>/${project.artifactId}</path>-->
      <path>/</path>
      <port>80</port>
    </configuration>
  </plugin>
  <!--  maven的源码打包插件 -->
  <plugin>
    <artifactId>maven-source-plugin</artifactId>
    <version>2.4</version>
    <executions>
      <execution>
        <phase>package</phase>
        <goals>
          <goal>jar-no-fork</goal>
        </goals>
      </execution>
    </executions>
  </plugin>

  <!-- 资源文件拷贝插件 -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-resources-plugin</artifactId>
    <version>2.7</version>
    <executions>
      <execution>
        <id>copy-xmls</id>
        <phase>process-sources</phase>
        <goals>
          <goal>copy-resources</goal>
        </goals>
        <configuration>
          <outputDirectory>${basedir}/target/classes</outputDirectory>
          <resources>
            <resource>
              <directory>${basedir}/src/main/java</directory>
              <includes>
                <include>**/*.xml</include>
              </includes>
            </resource>
            <resource>
              <directory>${basedir}/src/main/resources</directory>
              <includes>
                <include>**/*.xml</include>
              </includes>
            </resource>
          </resources>
        </configuration>
      </execution>
    </executions>
  </plugin>
</plugins>
```



# 四、**Maven构建SSM工程[应用]**

l 需求：SSM整合实现查询商品列表

l 整合思路回顾

SSM=(Mybatis+Spring)+SpringMVC

​	=(Dao层+Service层)+Web层

Dao层和Service分别开发完毕之后，然后按照三个整合目标完成后续工作：

目标1：数据库连接池和数据库事务交给Spring管理

目标2：SqlSessionFactory作为单例交给Spring管理

目标3：Spring扫描Mapper接口创建并管理Mapper代理对象，我们使用的时候直接去容器中拿到Mapper代理对象进行使用

测试Mybatis+Spring

 

再整合SpringMVC（相当于在已有整合工程的基础上开发SpringMVC的入门案例）

细节1：Controller层扫描应该配置在Springmvc的配置文件中

细节2：Spring框架使用监听器启动（配置在web.xml中）

l POM.xml

注意：以后到公司使用Maven也是这样，引入一个Jar包需要三步：定义版本、锁定版本、引入jar包；但是这么干未免太过麻烦，所以每个公司都有自己的POM文件模板，项目经理会提供给你你直接使用即可，比如像我们今天的项目就需要一个这样的模板

## （一）**依赖传递[理解]**

在Maven的POM文件中导入一个jar的坐标，那么Maven会把这个jar所依赖的jar全部下载下来，这种机制就叫做**依赖传递**。

​	我们**直接导入的jar叫做直接依赖**；由Maven补全的jar叫做**间接依赖**。

## （二）**依赖冲突的解决[理解]**

l 第一声明者优先原则：哪个jar坐标导入在前（pom文件中该jar的坐标配置在上面），依赖jar的版本和这个jar的版本保持一致

l 路径近者优先原则：在pom.xml中明确配置了某一个jar的版本（明确告诉maven我要使用哪个版本，那就是用哪个版本），那么就以此为准

l 版本锁定：**好处在父子工程中体现的更明显**

## （三）**搭建SSM工程[应用]**

### 1．**新建一个 ssm_maven 项目,使用下图选中的骨架**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps42.jpg) 

### 2．**下一步填写坐标**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps43.jpg) 

### 3．**确认是否使用的自己的私服，然后直接下一步完成**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps44.jpg) 

### 4．**在 main 目录下新建 java 和 resources 文件夹**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps45.jpg) 

### 5．**把 java 和 resources 文件夹转成 source root**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps46.jpg) 

### 6．**修改编译版本，在 pom.xml 文件中添加**

```xml
<build>

<plugins>

<!-- 设置编译版本为 1.8 -->

<plugin>

<groupId>org.apache.maven.plugins</groupId>

<artifactId>maven-compiler-plugin</artifactId>

<version>3.1</version >

<configuration>

<source>1.8</source>

<target>1.8</target>

<encoding>UTF-8</encoding>

</configuration>

</plugin>

</plugins>

</build>
```



### 7．**定义 pom.xml**

maven 工程首先要识别依赖，web 工程实现 SSM 整合，需要依赖 spring-webmvc5.0.2、

spring5.0.2、mybatis3.4.5 等，在 pom.xml 添加工程如下依赖：（在实际企业开发中会有专门得人来编写 pom.xml）

分两步：

1）锁定依赖版本

2）添加依赖

 ```xml
<groupId>com.mavenDemo.maven</groupId>
  <artifactId>ssm_maven</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>
  <!-- POM文件模板开始 -->
  <properties>
    <!-- 项目编码 -->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!-- 编译及输出的时候应用那个版本的jdk -->
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
    <!-- 定义spring的版本 -->
    <spring.version>5.0.2.RELEASE</spring.version>
  </properties>

  <dependencyManagement>
    <dependencies>
      <!-- 底层容器 -->
      <!-- SpringBean管理 -->
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <!-- Spring IOC管理 -->
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <!-- Spring AOP管理 -->
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aop</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <!-- 切面功能包里面包含了我们用的切面切入点织入等功能-->
      <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.8.7</version>
      </dependency>
      <!-- 数据库操作 -->
      <!-- mybatis的核心包 -->
      <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.4.5</version>
      </dependency>
      <!-- mybatis与spring的整合包 -->
      <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>1.3.1</version>
      </dependency>
      <!-- 事务操作 -->
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <!-- JDBC辅助 -->
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <!-- 页面功能 -->
      <!-- SpringMVC -->
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <!-- ServletAPI -->
      <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.0</version>
        <scope>provided</scope>
      </dependency>
      <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
        <scope>provided</scope>
      </dependency>
      <!-- JSTL -->
      <dependency>
        <groupId>jstl</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
      </dependency>
      <dependency>
        <groupId>taglibs</groupId>
        <artifactId>standard</artifactId>
        <version>1.1.2</version>
      </dependency>
      <!-- 单元测试 -->
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
      </dependency>
    </dependencies>
  </dependencyManagement>

  <dependencies>
    <!-- 底层容器 -->
    <!-- SpringBean管理 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
    </dependency>
    <!-- Spring IOC管理 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
    </dependency>
    <!-- Spring AOP管理 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aop</artifactId>
    </dependency>
    <!-- 切面功能包里面包含了我们用的切面切入点织入等功能-->
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
    </dependency>
    <!-- 数据库操作 -->
    <!-- mybatis的核心包 -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
    </dependency>
    <!-- mybatis与spring的整合包 -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
    </dependency>
    <!-- 事务操作 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
    </dependency>
    <!-- JDBC辅助 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
    </dependency>
    <!-- 页面功能 -->
    <!-- SpringMVC -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
    </dependency>
    <!-- ServletAPI -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jsp-api</artifactId>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
    </dependency>
    <!-- JSTL -->
    <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
    </dependency>
    <dependency>
      <groupId>taglibs</groupId>
      <artifactId>standard</artifactId>
    </dependency>
    <!-- 单元测试 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
    </dependency>
    <!-- 数据库驱动 -->
    <!-- Oracle驱动 -->
    <dependency>
      <groupId>com.oracle</groupId>
      <artifactId>ojdbc14</artifactId>
      <version>10.2.0.4.0</version>
    </dependency>
    <!-- mysql驱动 -->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.7</version>
    </dependency>
    <!-- 数据源 -->
    <dependency>
      <groupId>commons-dbcp</groupId>
      <artifactId>commons-dbcp</artifactId>
      <version>1.4</version>
    </dependency>
  </dependencies>
  <!-- POM文件模板结束 -->
 ```



### 8．**Dao 层**

在src/main/java 中定义 dao 接口，实现查询全部：

#### （1）**pojo 模型类**

在src/main/java 创建模型类

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps47.jpg) 

 

#### （2） **dao 层代码**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps48.jpg) 

配置文件：注意配置文件的位置

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps49.jpg) 

在 src/main/resources 创建 spring-core.xml

```xml
<!-- 1. 引入外部的配置文件 -->
    <context:property-placeholder location="classpath:ssm.properties" />
    <!-- 2. 配置注解bean的包扫描 -->
    <context:component-scan base-package="cn.mavenDemo">
        <!-- 排除springmvc的Controller注解 -->
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    </context:component-scan>
    <!-- 3. 配置数据源 -->
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
        <!-- 四大属性 -->
        <property name="driverClassName" value="${jdbc.driver}" />
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}" />
        <property name="password" value="${jdbc.userpass}" />
    </bean>
    <!-- 4.session 工厂 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 绑定数据源 -->
        <property name="dataSource" ref="dataSource" />
        <!-- 加载配置文件 -->
        <property name="configLocation" value="classpath:SqlMapConfig.xml" />
        <!-- 加载映射文件 -->
        <property name="mapperLocations" value="classpath:com/**/*Mapper.xml" />
    </bean>
    <!-- 5.mybaits的专用接口扫描 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="cn.mavenDemo.ssm.dao" />
    </bean>
    <!-- 6.事务管理器 -->
    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!-- 绑定数据源 -->
        <property name="dataSource" ref="dataSource" />
    </bean>
    <!-- 7.声明式事务 -->
    <tx:advice id="txAdvice" transaction-manager="txManager">
        <tx:attributes>
            <!-- 查询方法 : 只读事务,当前有事务沿用当前的事务,没事务以非事务方式运行,保证快 -->
            <tx:method name="find*" read-only="true" propagation="SUPPORTS"/>
            <!-- 操作方法 : 非只读,以读已提交数据为事务隔离界别,如果有事务用当前事务,没事创建一个新的始终保持在事务方法中运行 -->
            <!-- READ_COMMITTED 读已提交 并发访问速度快 但是不安全 牺牲安全保证速度 -->
            <tx:method name="insert*" isolation="READ_COMMITTED" />
            <tx:method name="update*" isolation="READ_COMMITTED" />
            <tx:method name="delete*" isolation="READ_COMMITTED" />
            <tx:method name="*" read-only="true" propagation="SUPPORTS"/>
        </tx:attributes>
    </tx:advice>
    <!-- 8.AOP 代理实现 -->
    <aop:config>
        <!-- 配置切入点 -->
        <aop:pointcut id="serviceCut" expression="execution(* cn.mavenDemo.ssm.service..*.*(..))" />
        <!-- 织入 -->
        <aop:advisor advice-ref="txAdvice" pointcut-ref="serviceCut" />
    </aop:config>
```



在 src/main/resources 创建SqlMapConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 1. 显示SQL语句 -->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    <!-- 2. 配置别名包 -->
    <typeAliases>
        <package name="cn.mavenDemo.pojo" />
    </typeAliases>
</configuration>  
```



创建ssm.properties

```xml
jdbc.driver = oracle.jdbc.OracleDriver
jdbc.url = jdbc:oracle:thin:@127.0.0.1:1521:orcl
jdbc.username = ssmoperate
jdbc.userpass = orcl
```



#### （3）**Service 层**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps50.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps51.jpg) 

#### （4）**Web 层**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps52.jpg) 

 

配置文件

在 src/main/resources 创建 spring-mvc.xml

 ```xml
<!-- 1.配置controller注解 -->
    <context:component-scan base-package="cn.mavenDemo">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    </context:component-scan>
    <!-- 2.加载预置的SpringMVC -->
    <mvc:annotation-driven />
    <!-- 3.配置视图解析器/WEB-INF/pages/account_list.jsp  -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 前缀 -->
        <property name="prefix" value="/WEB-INF/pages/" />
        <!-- 后缀 -->
        <property name="suffix" value=".jsp" />
    </bean>
 ```

Web.xml

加载 spring 容器，配置 springmvc 前端控制器

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <!-- 加载spring配置 -->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring-core.xml</param-value>
  </context-param>
  <!-- 过滤器 -->
  <filter>
    <filter-name>EncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter>
    <filter-name>RestFulFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>EncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
  <filter-mapping>
    <filter-name>RestFulFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>


  <!-- 配置spring的监听器 -->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <!-- springmvc核心servlet -->
  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- 加载一下springmvc的配置文件 -->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

</web-app>
```



#### （5）**JSP**

Index.jsp

```html
<%@ page isELIgnored="false" contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>系统首页</title>
    <script type="text/javascript">
        function goView() {
            window.location.href = "${pageContext.request.contextPath}/AccountViewAction/AccountView";
        }
        function goAdd() {
            alert("准备添加");
        }
    </script>
</head>
<body>
    <div>
        <h1>欢迎使用账户信息管理系统</h1>
        <h3>请选择你要使用的功能 :</h3>
        <input type="button" value="添加" onclick="goAdd();" />
        &nbsp;&nbsp;&nbsp;&nbsp;
        <input type="button" value="查询" onclick="goView();" />
    </div>
</body>
</html>
```



AccountList.jsp

```html
<%@ page isELIgnored="false" contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<html>
<head>
    <title>数据展示</title>
</head>
<body>
    <div>
        <c:if test="${not empty accounts}">
            <table border="1">
                <thead>
                    <tr>
                        <th>编号</th>
                        <th>姓名</th>
                        <th>金额</th>
                    </tr>
                </thead>
                <tbody>
                    <c:forEach items="${accounts}" var="a">
                        <tr align="center">
                            <td>${a.id}</td>
                            <td>${a.name}</td>
                            <td>
                                <fmt:formatNumber type="currency" value="${a.money}" />
                            </td>
                        </tr>
                    </c:forEach>
                </tbody>
            </table>
        </c:if>

    </div>
</body>
</html>
```



#### （6）**运行与调试**

已添加 tomcat7 插件，双击右侧 tomcat7 运行

|      |                                                              |
| ---- | ------------------------------------------------------------ |
|      | ![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps53.jpg) |

 

# 五、**分模块构建工程[应用]**

l 分析

 ![1576484075873](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576484075873.png)

l 分模块开发细节注意

注意：表现层工程web.xml去加载Spring applicationContext开头的配置文件时，我们要使用classpath*

l 理解继承和聚合

**通常继承和聚合同时使用**

**父工程的特征：packing： pom , （jar/war）**

n 何为继承？

继承是为了消除重复，如果将dao、service、web分开创建独立的工程，则每个工程的pom.xml文件中内容存在重复，比如设置编译版本、锁定spring的版本等，可以将这些重复的内容提取出放在父工程的pom.xml中定义

l 何为聚合？

项目开发通常分模块/分工程开发，每个模块开发完成要运行整个工程需要将每个模块聚合在一起，比如：dao、service、web三个工程最终打成一个war包运行

l 依赖范围对传递依赖的影响（了解）

service层依赖于dao层，有一个scope（runtime）

假如dao层依赖于一个jar包A，也有一个scope（test）配置

service能否使用A这个jar？看两个scope的值

![1576484108114](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576484108114.png)

 

## （一）**搭建分模块工程**

基于上边的三个工程分析

继承：创建一个 parent 工程将所需的依赖都配置在 pom 中

聚合：聚合多个模块运行。

### 1．**需求**

需求描述——将 SSM 工程拆分为多个模块开发：

ssm_dao		ssm_service		ssm_web

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps56.jpg) 

### 2．**创建父工程maven-parent**

#### （1）**选择骨架创建父工程**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps57.jpg) 

注意代码所在的路径

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps58.jpg) 

添加项目的打包方式

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps59.jpg) 

定义 pom.xml

在父工程的 pom.xml 中抽取一些重复的配置的，比如：锁定 jar 包的版本、设置编译版本等。

 ```xml
<properties>

<spring.version>5.0.2.RELEASE</spring.version>

<springmvc.version>5.0.4.RELEASE</springmvc.version> <mybatis.version>3.4.5</mybatis.version>

</properties>



<dependencyManagement>

<dependencies>

<!-- Mybatis -->

<dependency>

<groupId>org.mybatis</groupId>

<artifactId>mybatis</artifactId>

<version>${mybatis.version}</version> </dependency>

<!-- springMVC -->

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-webmvc</artifactId> <version>${springmvc.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-context</artifactId> <version>${spring.version}</version>

</dependency>

<!-- spring -->

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-core</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-aop</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId><artifactId>spring-web</artifactId>

<version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-expression</artifactId> <version>${spring.version}</version>

</dependency>



<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-beans</artifactId> <version>${spring.version}</version>

</dependency>



<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-aspects</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-context-support</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-test</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-jdbc</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-tx</artifactId>

<version>${spring.version}</version>

</dependency>
</dependencies>

</dependencyManagement>

<build>

<plugins>

<plugin>

<groupId>org.apache.maven.plugins</groupId>

<artifactId>maven-compiler-plugin</artifactId>

<version>3.1</version>

<configuration>

<target>1.8</target>

<source>1.8</source>

</configuration>

</plugin>

</plugins>

</build>
 ```

 

将父工程发布至仓库

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps60.jpg) 

#### （2）**ssm_dao 子模块**

在父工程上右击创建 maven 模块：

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps61.jpg) 

选择“跳过骨架选择”

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps62.jpg) 

 

填写模块名称

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps63.jpg) 

 

下一步，确定项目的目录

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps64.jpg) 

添加打包方式是 jar

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps65.jpg) 

 

定义 pom.xml

 ```xml
<dependencies>

<!-- Mybatis 和 mybatis 与 spring 的整合 --> <dependency>

<groupId>org.mybatis</groupId>

<artifactId>mybatis</artifactId>

</dependency>

<dependency>

<groupId>org.mybatis</groupId>

<artifactId>mybatis-spring</artifactId> <version>1.3.1</version>

</dependency>

<!-- MySql 驱动 -->

<dependency>

<groupId>mysql</groupId>

<artifactId>mysql-connector-java</artifactId> <version>5.1.32</version>

</dependency>
<!-- druid 数据库连接池 -->

<dependency>

<groupId>com.alibaba</groupId>

<artifactId>druid</artifactId>

<version>1.0.9</version>

</dependency>

<!-- spring 相关 -->

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-context</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-core</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-aop</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-web</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-expression</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-beans</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-aspects</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId><artifactId>spring-context-support</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-test</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-jdbc</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-tx</artifactId>

</dependency>

</dependencies>
 ```



只添加到层的 pom，mybatis 和 spring 的整合相关依赖

 

 

dao 模块代码编写（省略）

把 dao 模块 install 到本地仓库（省略）

#### （3）**ssm_service 子模块（参考dao）**

定义 pom.xml

ssm_service 模块的 pom.xml 文件中需要继承父模块，ssm_service 依赖 ssm_dao 模块，添加spring 相关的依赖：

 ```xml
<dependencies>

<!-- spring 相关 -->

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-jdbc</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-tx</artifactId>

</dependency>

<!--	dao 层的依赖 -->

<dependency>

<groupId>com.mavenTest.ssm_mavenTestt</groupId>

<artifactId>ssm_dao</artifactId>

<version>1.0-SNAPSHOT</version></dependency>

</dependencies>
 ```



把 service 模块 install 到本地仓库（省略）

 

#### （4）**ssm_web 子模块**

选择骨架创建 web 子模块

|      |                                                              |
| ---- | ------------------------------------------------------------ |
|      | ![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps66.jpg) |

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps67.jpg) 

定义模块名称，下一步确认使用自己的本地仓库

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps68.jpg) 

**使用骨架创建** **web** **项目会花费些时间，请耐心等待**

 

创建 java 和 resources 文件夹，转成 source root

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps69.jpg) 

 

定义 pom.xml

ssm_web 模块的 pom.xml 文件中需要继承父模块，ssm_web 依赖 ssm_service 模块,和springmvc 的依赖

 ```xml
<dependencies>

<!-- 依赖 service -->

<dependency>

<groupId>com.mavenTest.ssm_mavenTestt</groupId>

<artifactId>ssm_service</artifactId> <version>1.0-SNAPSHOT</version>

</dependency>

<!-- springMVC -->

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-webmvc</artifactId> <version>${springmvc.version}</version>

</dependency>

<dependency>

<groupId>javax.servlet</groupId>

<artifactId>servlet-api</artifactId> <version>2.5</version> <scope>provided</scope>

</dependency>

<dependency>

<groupId>javax.servlet</groupId>

<artifactId>jsp-api</artifactId>
<version>2.0</version>

<scope>provided</scope>

</dependency>

<!-- jstl -->

<dependency>

<groupId>javax.servlet</groupId>

<artifactId>jstl</artifactId>

<version>1.2</version>

</dependency>

</dependencies>
 ```

controller代码编写和配置文件（省略）

 

注意 web.xml 中的配置：使用通配符加载

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps70.png) 

#### （5）**运行调试**

方法 1：在 ssm_web 工程的 pom.xml 中配置 tomcat 插件运行

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/wps71.png) 

运行 ssm_web 工程它会从本地仓库下载依赖的 jar 包，所以当 ssm_web 依赖的 jar 包内容修改了必须及时发布到本地仓库，比如：ssm_web 依赖的 ssm_service 修改了，需要及时将ssm_service 发布到本地仓库。

 

方法 2：在父工程的 pom.xml 中配置 tomcat 插件运行，自动聚合并执行

 

推荐方法 2，如果子工程都在本地，采用方法 2 则不需要子工程修改就立即发布到本地仓库，父工程会自动聚合并使用最新代码执行。

 

注意：如果子工程和父工程中都配置了 tomcat 插件，运行的端口和路径以子工程为准。

 

### 3．**分模块构建工程-依赖整合**

每个模块都需要 spring 或者 junit 的 jar，况且最终 package 打完包最后生成的项目中的

jar 就是各个模块依赖的整合，所以我们可以把项目中所需的依赖都可以放到父工程中,模块

中只留模块和模块之间的依赖，那父工程的 pom.xml 可以如下配置：

```xml
<properties>

<spring.version>5.0.2.RELEASE</spring.version>

<springmvc.version>5.0.2.RELEASE</springmvc.version>

<mybatis.version>3.4.5</mybatis.version>

</properties>



<dependencyManagement>

<dependencies>

<!-- Mybatis -->

<dependency>

<groupId>org.mybatis</groupId>

<artifactId>mybatis</artifactId>

<version>${mybatis.version}</version>

</dependency>

<!-- springMVC -->

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-webmvc</artifactId>

<version>${springmvc.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-context</artifactId>

<version>${spring.version}</version>

</dependency>

<!-- spring -->

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-core</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>
<groupId>org.springframework</groupId> <artifactId>spring-aop</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-web</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-expression</artifactId> <version>${spring.version}</version>

</dependency>
<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-beans</artifactId> <version>${spring.version}</version>

</dependency>
<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-aspects</artifactId> <version>${spring.version}</version>

</dependency>
<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-context-support</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-test</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId><groupId>org.springframework</groupId> <artifactId>spring-aop</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-web</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-expression</artifactId> <version>${spring.version}</version>

</dependency>
<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-beans</artifactId> <version>${spring.version}</version>

</dependency>
<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-aspects</artifactId> <version>${spring.version}</version>

</dependency>
<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-context-support</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-test</artifactId> <version>${spring.version}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId></dependency>

<!-- spring 相关 -->

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-context</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-core</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-aop</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-web</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-expression</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-beans</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-aspects</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-context-support</artifactId>

</dependency>
<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-test</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-jdbc</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-tx</artifactId>

</dependency>

<!-- spring 相关 事务相关 -->

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-jdbc</artifactId>

</dependency>

<dependency>

<groupId>org.springframework</groupId> <artifactId>spring-tx</artifactId>

</dependency>

<!-- junit 测试 -->

<dependency>

<groupId>junit</groupId>

<artifactId>junit</artifactId>

<version>4.12</version>

<scope>test</scope>

</dependency>


<dependency>

<groupId>javax.servlet</groupId>

<artifactId>servlet-api</artifactId> <version>2.5</version> <scope>provided</scope>

</dependency>

<dependency>

<groupId>javax.servlet</groupId>

<artifactId>jsp-api</artifactId>

<version>2.0</version><scope>provided</scope>

</dependency>

<!-- jstl -->

<dependency>

<groupId>javax.servlet</groupId>

<artifactId>jstl</artifactId>

<version>1.2</version>

</dependency>

</dependencies>


<build>

<plugins>

<plugin>

<groupId>org.apache.maven.plugins</groupId>

<artifactId>maven-compiler-plugin</artifactId>

<version>3.1</version>

<configuration>

<target>1.8</target>

<source>1.8</source>

</configuration>

</plugin>

<plugin>

<groupId>org.apache.tomcat.maven</groupId>

<artifactId>tomcat7-maven-plugin</artifactId> <version>2.2</version>

</plugin>

</plugins>

</build>
```



 

# 六、**Maven私服[了解]**

公司在自己的局域网内搭建自己的远程仓库服务器，称为私服（私有服务器），私服服务器即是公司内部的maven远程仓库，每个员工的电脑上安装maven软件并且连接私服服务器，员工将自己开发的项目打成jar并发布到私服服务器，其它项目组从私服服务器下载所依赖的构件（jar）。 

Maven私服作用：

1、我们不能联网的情况下，它帮我们从互联网下载jar（私服中jar的来源1）

2、自己开发的jar上传到私服供（共享给）团队使用（私服中jar的来源2）

3、最终私服就是让我们下载jar的

 ![1576484555774](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576484555774.png)

私服还充当一个代理服务器，当私服上没有jar包会从互联网中央仓库自动下载。

 ![1576484573957](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576484573957.png)

Maven服务器：某一个机器上安装了可以提供Maven私服功能的软件，那么这个机器就叫做Maven私有服务器。

## （一）**搭建私服环境**

**nexus**是Maven仓库管理器(**maven私服软件**)，通过nexus可以搭建maven私服仓库，同时nexus还提供强大的仓库管理功能等。

l 安装nexus

n 解压nexus-2.12.0-01-bundle.zip（无中文和空格路径下），进入bin目录

![1576484602741](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576484602741.png)

n 管理员身份运行cmd，进入bin目录，执行nexus.bat install（若安装后卸载可执行nexus.bat uninstall）

![1576484619603](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576484619603.png)

n services.msc进入服务管理，查看nexus服务

![1576484636186](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576484636186.png)

n 若nexus服务已经安装，可以使用两种方式启动nexus

u bin目录下执行nexus.bat start

u 直接启动nexus服务

n 查看nexus的配置文件conf/nexus.properties

![1576484679729](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576484679729.png)

n 访问<http://localhost:8081/nexus/>   内置登录名：admin/admin123

![1576484697831](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576484697831.png)

n 仓库类型

![1576484714210](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576484714210.png)

nexus的仓库有4种类型： 

1. hosted，**宿主仓库**，部署自己的jar到这个类型的仓库，包括releases和snapshot两部分，Releases公司内部发布版本仓库、 Snapshots 公司内部测试版本仓库 

2. proxy，代理仓库，用于代理远程的公共仓库，如maven中央仓库，用户连接私服，私服自动去中央仓库下载jar包或者插件。 

3. group，仓库组，用来合并多个hosted/proxy仓库，通常我们配置自己的maven连接仓库组。 

4. virtual(虚拟)：兼容Maven1 版本的jar或者插件 

**接下来三件事：**

1、 把我们的项目发布到私服；

2、 从私服上下载jar；

3、 如何向私服上上传第三方的jar

## （二）**项目发布到nexus私服**

对远程的一个操作：**url、用户名、密码**都得有，后续上传就是围绕这三个信息进行；

 

第一步：maven软件settings.xml中配置连接私服的用户名和密码（放在servers标签内）

```xml
<!--配置用户名密码开始-->
      <server>
              <id>releases</id>
              <username>admin</username>
              <password>admin123</password>
            </server>
            <server>
              <id>snapshots</id>
              <username>admin</username>
              <password>admin123</password>
        </server>
        <!--配置用户名密码结束-->
```

第二步：配置项目的pom.xml

```xml
<!--添加到要上传的项目pom中确定上传路径开始-->
         <distributionManagement>
              <repository>
                  <id>releases</id>
            <url>http://localhost:8081/nexus/content/repositories/releases/</url>
              </repository> 
              <snapshotRepository>
                  <id>snapshots</id>
            <url>http://localhost:8081/nexus/content/repositories/snapshots/</url>
              </snapshotRepository> 
          </distributionManagement>
        <!--添加到要上传的项目pom中确定上传路径结束-->
```



注意：pom.xml这里<id> 和 settings.xml 配置 <id> 对应

第三步：执行mvn deploy命令

## （三）**从nexus私服下载jar**

在maven的settings.xml文件中配置下载模板（放在profiles标签内）

```xml
<profile>   
            <!--profile的id-->
           <id>dev</id>   
            <repositories>   
              <repository>  
                <!--仓库id，repositories可以配置多个仓库，保证id不重复-->
                <id>nexus</id>   
                <!--仓库地址，即nexus仓库组的地址-->
                <url>http://localhost:8081/nexus/content/groups/public/</url>   
                <!--是否下载releases构件-->
                <releases>   
                  <enabled>true</enabled>   
                </releases>   
                <!--是否下载snapshots构件-->
                <snapshots>   
                  <enabled>true</enabled>   
                </snapshots>   
              </repository>   
            </repositories>  
             <pluginRepositories>  
                <!-- 插件仓库，maven的运行依赖插件，也需要从私服下载插件 -->
                <pluginRepository>  
                    <!-- 插件仓库的id不允许重复，如果重复后边配置会覆盖前边 -->
                    <id>public</id>  
                    <name>Public Repositories</name>  
                    <url>http://localhost:8081/nexus/content/groups/public/</url>  
                </pluginRepository>  
            </pluginRepositories>  
          </profile>  
```



激活下载模板配置

```xml
<!--激活模板开始-->
          <activeProfiles>
            <activeProfile>dev</activeProfile>
          </activeProfiles>
        <!--激活模板结束-->
```



测试：删除本地的dao工程，终端进入service工程下执行mvn compile

![1576484833189](https://github.com/lagouedu/Basic-document/blob/master/img-folder/Maven/1576484833189.png)

## （四）**把第三方jar放入本地仓库或者私服**

第三方jar放入本地仓库

```xml
 mvn install:install-file -Dfile=ojdbc14-10.2.0.4.0.jar -DgroupId=com.oracle -DartifactId=ojdbc14 -Dversion=10.2.0.4.0 -Dpackaging=jar
```

第三方jar放入私服

maven的settings配置文件中配置第三方仓库的server信息

```xml
<server> 
<id>thirdparty</id> 
<username>admin</username> 
<password>admin123</password> 
</server>
```

执行命令

```xml
mvn deploy:deploy-file -Dfile=ojdbc14-10.2.0.4.0.jar -DgroupId=com.oracle -DartifactId=ojdbc14 -Dversion=10.2.0.4.0 -Dpackaging=jar -Durl=http://localhost:8081/nexus/content/repositories/thirdparty/ -DrepositoryId=thirdparty
```

