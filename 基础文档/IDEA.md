# **IDEA开发工具**

# 一、**编码神器-IDEA**

## （一）**IDEA介绍[了解]**

l 官网：[http://www.jetbrains.com/idea/download/#section=windows](#section=windows) 

l 官网教程: 

<https://www.jetbrains.com/help/idea/install-and-set-up-product.html>

l 百科介绍

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps115.jpg) 

l IDEA是 JetBrains 公司的产品，公司旗下还有其它产品，比如：

Ø WebStorm：用于开发 JavaScript、HTML5、CSS3 等前端技术；

Ø PyCharm：用于开发 python

Ø PhpStorm：用于开发 PHP

Ø RubyMine：用于开发 Ruby/Rails

Ø AppCode：用于开发 Objective - C/Swift	

Ø CLion：用于开发 C/C++

Ø DataGrip：用于开发数据库和 SQL

Ø Rider：用于开发.NET

Ø GoLand：用于开发 Go

Ø Android Studio：用于开发 android

l IDEA，全称 IntelliJ IDEA，是 Java 语言的集成开发环境，IDEA 在业界被公认为是最好的 java 开发工具之一，尤其在智能代码助手、代码自动提示、Debug调试、重构、J2EE支持、Ant、JUnit、CVS 整合、代码审查、创新的 GUI 设计等方面的功能可以说是超常的。

IntelliJ IDEA 在 2015 年的官网上这样介绍自己：

Excel at enterprise, mobile and web development with Java, Scala and Groovy,with all the latest modern technologies and frameworks available out of thebox.

翻译：

IntelliJ IDEA 主要用于支持 Java、Scala、Groovy 等语言的开发工具，同时具备支持目前主流的技术和框架，擅长于企业应用、移动应用和 Web 应用的开发。

l 注意: IntelliJ IDEA 对硬件的要求

2G 内存的计算机只适合写小程序、小项目或是开发静态页面。

Java Web 项目最好的方案是 8G 内存或是以上，硬盘能再用上固态是最好的，因为IntelliJ IDEA 有大量的缓存、索引文件，把 IntelliJ IDEA 的缓存、索引文件放在固态上，IntelliJ IDEA 流畅度也会加快很多。

## （二）**下载-安装[掌握]**

### 1．**下载**

[https://www.jetbrains.com/idea/download/#section=windows](#section=windows)

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps116.jpg) 

 

### 2．**安装**

#### （1）**点击安装**

l 本教程所对应的版本2017.3.2版本

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps117.jpg) 

**注意:所有软件安装路径中不能出现空格和中文！！！**   

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps118.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps119.jpg) 

l 在桌面上创建一个快捷图标，建议勾选上，方便我们在安装后定位 IntelliJ IDEA 安装目录。

l 关联 Java 和 Groovy 文件，建议都不要勾选，正常我们会在 Windows 的文件系统上打开这类文件都是为了快速查阅文件里面的内容，如果用 IntelliJ IDEA 关联上之后，由于 IntelliJ IDEA 打开速度缓慢，这并不能方便我们查看

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps120.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps121.jpg) 

#### （2）**运行并破解**

百度idea服务器破解或者idea破解码

#### （3）**选择UI主题**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps122.jpg) 

#### （4）**选择插件(跳过)**

这里可以按默认设置，点击下一步，后续可以在settings中修改

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps123.jpg) 

### 3．**目录介绍**

#### （1）**安装目录**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps124.jpg) 

bin：容器，执行文件和启动参数等

help：快捷键文档和其他帮助文档

jre64：64 位java 运行环境

lib：idea 依赖的类库

license：各个插件许可

plugins：插件

其中：bin 目录下：

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps125.jpg) 

• idea.exe 文件是 IntelliJ IDEA 32 位的可行执行文件，IntelliJ IDEA 安装完默认发送到桌面的也就是这个执行文件的快捷方式。

• idea.exe.vmoptions 文件是 IntelliJ IDEA 32 位的可执行文件的 VM 配置文件

• idea64.exe 文件是 IntelliJ IDEA 64 位的可行执行文件，要求必须电脑上装有 JDK 64 位版本。64 位的系统也是建议使用该文件。

• idea64.exe.vmoptions 文件是 IntelliJ IDEA 64 位的可执行文件的 VM 配置文件

• idea.properties 文件是 IntelliJ IDEA 的一些属性配置文件

 

配置文件修改的原则主要是根据自己机器的内存情况来判断的，个人是建议 8G 以下的机子或是静态页面开发者都是无需修改的。如果你是开发大型项目、Java 项目或是 Android 项目，并且内存大于 8G，建议进行修改

-Xms128m，16 G 内存的机器可尝试设置为 -Xms512m	(设置初始的内存数，增加该值可以提高 Java 程序的启动速度。)

-Xmx750m，16 G 内存的机器可尝试设置为 -Xmx1500m	(设置最大内存数，提高该值，可以减少内存 Garage 收集的频率，提高程序性能)

-XX:ReservedCodeCacheSize=240m，16G 内存的机器可尝试设置为-XX:ReservedCodeCacheSize=500m	(缓存大小)

#### （2）**配置目录**

l 目录介绍

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps126.jpg) 

• 对于这个设置目录有一个特性，就是你删除掉整个目录之后，重新启动 IntelliJ IDEA 会再自动帮你再生成一个全新的默认配置，所以很多时候如果你把 IntelliJ IDEA 配置改坏了，没关系，删掉该目录，一切都会还原到默认，我是很建议新人可以多自己摸索 IntelliJ IDEA 的配置，多几次还原，有助于加深对 IntelliJ IDEA 的了解。

• config 目录是 IntelliJ IDEA 个性化化配置目录，或者说是整个 IDE 设置目录。也是我个人认为最重要的目录，没有之一，安装新版本的 IntelliJ IDEA 会自动扫描硬盘上的旧配置目录，指的就是该目录。这个目录主要记录了：IDE 主要配置功能、自定义的代码模板、自定义的文件模板、自定义的快捷键、Project 的 tasks 记录等等个性化的设置。

• system 目录是 IntelliJ IDEA 系统文件目录，是 IntelliJ IDEA 与开发项目一个桥梁目录，里面主要有：缓存、索引、容器文件输出等等，虽然不是最重要目录，但是也是最不可或缺目录之一。

l 配置文件的位置可以通过修改properties文件进行指定

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps127.jpg) 

• 上图是 IntelliJ IDEA 一些属性配置，没有 32 位和 64 位之分，修改原则主要根据个人对 IntelliJ IDEA 的个性化配置情况来分析。常修改的就是下面 4 个参数：

• idea.config.path=${user.home}/.IntelliJIdea/config ，该属性主要用于指向 IntelliJ IDEA 的个性化

配置目录，默认是被注释，打开注释之后才算启用该属性，这里需要特别注意的是斜杠方向，这里用的是正斜杠。

• idea.system.path=${user.home}/.IntelliJIdea/system ，该属性主要用于指向 IntelliJ IDEA 的系统文件目录，默认是被注释，打开注释之后才算启用该属性，这里需要特别注意的是斜杠方向，这里用的是正斜杠。如果你的项目很多，则该目录会很大，如果你的 C 盘空间不够的时候，还是建议把该目录转移到其他盘符下。

## （三）**入门案例[掌握]**

### 1．**创建普通Java工程**

#### （1）**创建工程并编写代码**

l 新建工程

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps128.jpg) 

•Create New Project 创建一个新项目。

•Import Project 导入一个已有项目。

•Open 打开一个已有项目。

•Check out from Version Control 可以通过服务器上的项目地址 Checkout Github 上面项目或是其他 Git 托管服务器上的项目。

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps129.jpg) 

l next

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps130.jpg) 

l 指定名称和位置

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps131.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps132.jpg) 

l 显示工具

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps133.jpg) 

l 界面说明

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps134.jpg) 

Ø 工程下的 src 类似于 Eclipse 下的 src 目录，用于存放代码。

Ø 工程下的.idea 和 .iml 文件都是 IDEA 工程特有的。类似于 Eclipse 工程下的.settings、.classpath、.project 等。

ü  .idea 即为 Project 的配置文件目录。

ü  .iml 即为 Module 的配置文件。

l 设置项目jdk环境

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps135.jpg) 

l 创建类

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps136.jpg)![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps137.jpg) 

l 注意：不管是创建 class，还是 interface，还是 annotation，都是选择 new – java class

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps138.jpg) 

l 编写代码

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps139.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps140.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps141.jpg) 

l 说明：在 IDEA 里写完代码，不用点击保存。

l 运行代码

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps142.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps143.jpg) 

注意：IntelliJ IDEA 是一个没有 Ctrl + S 的 IDE，所以每次修改完代码你只要管着运行或者调试即可，无需担心保存或者丢失代码。

#### （2）**添加jar包**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps144.jpg) 

### 2．**创建web工程**

中间件服务器：tomcat、weblogic、websphere、jettyjboss

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps145.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps146.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps147.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps148.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps149.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps150.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps151.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps152.jpg) 

现在基本的web项目是创建了，但是目录结构还有很多需要修改的。

l 创建WEB-INF目录

在web文件夹上右键-->New->Directory：WEB-INF

l 没有lib目录和classes目录

在WEB-INF文件夹上右键-->New->Directory:创建两个文件夹：classes和lib，结果如下：

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps153.jpg) 

l 没有web.xml问题解决

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps154.jpg) 

编译地址的修改：File-->Project Structure-->Modules-->Paths:选择Use module compile output path：选择classes目录

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps155.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps156.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps157.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps158.jpg) 

 ```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
/**
 * Created by caowei on 2018/8/10.
 */
@WebServlet("/hello")
public class MavenDemo1 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
      response.getWriter().print(new Date().toLocaleString());
    }
}
 ```

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps159.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps160.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps161.jpg) 

修改配置支持热部署

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps162.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps163.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps164.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps165.jpg) 

访问测试

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps166.jpg) 

或者在创建完web项目后再添加tomcat支持

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps167.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps168.jpg) 

## （四）**概念说明[了解]**

### 1．**图标**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps169.jpg) 

Source root说明：你可以理解为源目录，源码的作用就是用来专门放 Java 类文件，相对于编译出来的 class 文件而言，它就是源。我们一般默认名字叫 src 的目录就是源目录，但是其实并不是这样的，在 IntelliJ IDEA 中，即使叫 srcs 也是可以设置为 Source root ，所以源目录跟目录命名是没有关系的，而是在于 IntelliJ IDEA 支持对任意目录进行设置为 Source root ，作用是标记该目录下的文件是可编译的。

### 2．**Project和Module**

l 官网说明

<https://www.jetbrains.com/idea/help/eclipse.html>

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps170.jpg) 

IDEA  官网：

An Eclipse workspace is similar to a project in IntelliJ IDEA

An Eclipse project maps to a module in IntelliJ IDEA

翻译 ：

Eclipse  中 workspace 相当于 IDEA  中的 的 Project

Eclipse  中 Project 相当于 IDEA  中的 Module

l 说明

IntelliJ IDEA 是没有类似 Eclipse 的工作空间的概念（ Workspaces ），最大单元就是 Project 。如果你同时观察多个项目的情况，IntelliJ IDEA 提供的解决方案是打开多个项目实例，你可以理解为开多个项目窗口。

在 IntelliJ IDEA 中 Project 是最顶级的级别，次级别是 Module。一个 Project可以有多个 Module。

目前主流的大型项目结构都是类似这种多 Module 结构，这类项目一般是这样划分的，比如：core Module、web Module、plugin Module、solr Module 等等，模块之间彼此可以相互依赖。通过这些 Module 的命名也可以看出，他们之间应该都是处于同一个项目业务情况下的模块，彼此之间是有不可分割的业务关系的。

所以我们现在总结：一个 Project 是由一个或多个 Module 组成，模块之间尽量是处在同一个项目业务的的情况下，彼此之间互相依赖关联。这里用的是尽量 ，因为 IntelliJ IDEA 的 Project 是一个没有具备任何编码设置、构建等开发功能的，主要起到一个项目定义、范围约束、规范等类型的效果，也许我们可以简单地理解为就是一个单纯的目录，只是这个目录命名上必须有其代表性的意义

l 但是

我们可以通过使用Project管理Module的方式，实现类似Eclipse中Workspace的概念，即把 Project当作Eclipse中的Workspace使用，如:

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps171.jpg) 

l 新建空项目作为工作空间

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps172.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps173.jpg) 

新建module作为项目

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps174.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps175.jpg) 

最后可以实现像Eclipse一样的效果

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps176.jpg) 

l 注意：删除模块前需要先移除模块，然后去磁盘目录手动删除即可

## （五）**常见设置[应用]**

### 1．**显示工具栏图标**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps177.jpg) 

### 2．**查看项目配置(项目源目录、输出以及所依赖的jar****)**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps178.jpg) 

或者

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps179.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps180.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps181.jpg) 

### 3．**全局默认设置（配置针对整个Idea生效）**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps182.jpg) 

或者

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps183.jpg) 

### 4．**普通设置（针对Project级别，工作空间级别）**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps184.jpg) 

说明

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps185.jpg) 

注意：通过settings进行的普通设置有一些可以全局生效

### 5．**更改字体大小通过ctrl+鼠标滚轮**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps186.jpg) 

### 6．**代码提示不区分大小写**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps187.jpg) 

### 7．**修改主题**

l 修改主题

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps188.jpg) 

### 8．**修改字体(了解)**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps189.jpg) 

修改控制台输出字体

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps190.jpg) 

### 9．**修改控制台字体颜色(了解)**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps191.jpg) 

### 10．**修改注释的字体颜色(了解)**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps192.jpg) 

Ø Doc Comment – Text：修改文档注释的字体颜色

Ø Block comment(块注释)：修改多行注释的字体颜色

Ø Line comment：修改当行注释的字体颜色

### 11．**设置文件编码（重要）**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps193.jpg) 

说明：Transparent native-to-ascii conversion 主要用于转换 ascii，一般都要勾选，不然 Properties 文件中的注释显示的都不会是中文

### 12．**设置鼠标悬浮提示**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps194.jpg) 

### 13．**自动导包&优化导包**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps195.jpg) 

### 14．**设置自动编译**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps196.jpg) 

类似于Eclipse中的自动编译

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps197.jpg) 

### 15．**空格与tab缩进设置**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps198.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps199.jpg) 

### 16．**显示行号和方法分割线**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps200.jpg) 

### 17．**去除单词拼写检查**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps201.jpg) 

### 18．**清理缓存和索引**

随着项目越来越大，新编写的代码没有立即生效（引用等），此时可以清理缓存和索引

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps202.jpg) 

通过上面方式清除缓存、索引本质也就是去删除 C 盘下的 system 目录下的对应的文件而已，所以如果你不用上述方法也可以删除整个 system 。当 IntelliJ IDEA 再次启动项目的时候会重新创建新的 system 目录以及对应项目缓存和索引。

如果你遇到了因为索引、缓存坏了以至于项目打不开，那也建议你可以直接删除 system 目录，一般这样都可以很好地解决你的问题。

### 19．**设置目录折叠**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps203.jpg) 

### 20．**定位到当前文件所在目录**![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps204.jpg)

### 21．**配置文件的生成模板**

 ```java
/**
 * @author ${USER}
 * @date ${DATE} ${TIME}
 * @description 
 */
 ```

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps205.jpg) 

### 22．**Debug**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps206.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps207.jpg) 

### 23．**省电模式**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps208.png)	如上图标注 1 所示，IntelliJ IDEA 有一种叫做 省电模式 的状态，开启这种模式之后 IntelliJ IDEA 会关掉代码检查和代码提示等功能。所以一般我也会认为这是一种 阅读模式

如果你在开发过程中遇到突然代码文件不能进行检查和提示可以来看看这里是否有开启该功能。 

### 24．**显示内存占用**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps209.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps210.jpg) 

### 25．**设置水平或者垂直分屏编辑**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps211.jpg) 

 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps212.jpg) 

### 26．**设置启动idea或打开项目时提示**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps213.jpg) 

### 27．**设置代码环绕**

Ctrl + Alt + T 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps214.jpg) 

### 28．**取消更新**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps215.jpg) 

### 29．**查看本地文件编辑历史**

类似ctrl+z功能的升级

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps216.jpg) 

### 30．**生成 javadoc**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps217.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps218.jpg) 

zh_CN

-encoding UTF-8 -charset UTF-8

### 31．**自定义代码模版**

l 关于模板(Templates)

代码模板的原理就是配置一些常用代码字母缩写，在输入简写时可以出现你预定义的固定模式的代码，使得开发效率大大提高，同时也可以增加个性化。

最简单的例子就是在 Java 中输入 sout 会出现 System.out.println();

l Idea中代码模板有两类Live Templates和Postfix Completion

二者的区别：Live Templates 可以自定义，而 Postfix Completion 不可以。同时，有些操作二者都提供了模板，Postfix Templates 较 Live Templates 能快 0.01 秒

l 官方介绍 Live Templates：

<https://www.jetbrains.com/help/idea/using-live-templates.html>

l Postfix Completion

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps219.jpg) 

Live Templates

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps220.jpg) 

Itli遍历集合/itar遍历数组（使用较多）

二者的区别：有些操作二者都提供了模板，Postfix Templates 较 Live Templates 能快 0.01 秒

举例 ：

l psvm :  可生成 main  方法 

l sout : System.out.println()  快捷输出

类似的：

soutp=System.out.println("方法形参名 = " + 形参名);

soutv=System.out.println("变量名 = " + 变量);

soutm=System.out.println("当前类名.当前方法");

“abc”.sout => System.out.println("abc");

l fori :  可生成 for  循环

类似的：

iter：可生成增强 for 循环

itar：可生成普通 for 循环

l list.for :  可生成集合 list 的 的 for  循环

List<String> list = new ArrayList<String>();

输入: list.for 即可输出

for(String s:list){

}

又如：list.fori 或 list.forr

l ifn ：可生成 if(xxx = null)

类似的：

inn：可生成 if(xxx != null) 或 xxx.nn 或 xxx.null

l prsf ：可生成 private static final

类似的：

psf：可生成 public static final

psfi：可生成 public static final int

psfs：可生成 public static final String

 

经常使用的如下：

 ![1576482299207](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/1576482299207.png)

l 修改现有模板 修改现有模板:Live Templates

如果对于现有的模板，感觉不习惯、不适应的，可以修改：

修改 1 ：

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps222.jpg) 

通过调用 psvm 调用 main 方法不习惯，可以改为跟 Eclipse 一样，使用 main 调取。

修改 2 ：

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps223.jpg) 

类似的还可以修改 psfs。

l 自定义模板 自定义模板

IDEA 提供了很多现成的 Templates。但你也可以根据自己的需要创建新的Template。

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps224.jpg) 

先定义一个模板的组：

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps225.jpg) 

选中自定义的模板组，点击”+”来定义模板。

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps226.jpg) 

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps227.jpg) 

```java
@Test
public void test$VAR1$(){
       $END$ 
}
```

\1. Abbreviatio：模板的缩略名称

\2. Description：模板的描述

\3. Template text：模板的代码片段

\4. 应用范围：比如点击 Define，然后在 java 类文件中应用即可

 

类似的可以再配置自己喜欢的Template:

 

## （六）**IDEA快捷键[应用]**

### 1．**Idea原生快捷键**

 

| **快捷键**            | **介绍**                                                     |
| --------------------- | ------------------------------------------------------------ |
| Alt + Enter           | 根据光标处所在的问题，提供快速修复选择，光标放在的位置不同提示的结果也不同（万能修复快捷键，同时可以帮助我们生成本地变量==eclipse中的ctrl+1）注意：非个人编码问题导致的错误，都可以尝试使用该快捷键修复 |
| Alt + Insert          | 代码自动生成，如生成对象的 set / get 方法，构造函数，toString() 等 |
| Shift + Shift         | 查找所有文件                                                 |
| Ctrl + D              | 复制当前行到下一行                                           |
| Ctrl + Y              | 删除当前行                                                   |
| Alt + 左方向键        | 向左切换tab                                                  |
| Alt + 右方向键        | 向右切换tab                                                  |
| Ctrl + Alt + 左方向键 | 快速返回上次查看代码的位置（Back）                           |
| Ctrl + Alt + 右方向键 | 快速返回上次查看代码的位置（Forward）                        |
| Ctrl + F              | 在当前文件进行文本查找                                       |
| Ctrl + Shift + F      | 在当前项目进行文本查找                                       |
| Ctrl + R              | 在当前文件进行文本替换                                       |
| Ctrl + Shift + R      | 在当前项目进行文本替换                                       |
| Ctrl + /              | 注释光标所在行代码，会根据当前不同文件类型使用不同的注释符号 |
| Ctrl + Shift + /      | 代码块注释                                                   |
| Ctrl + Alt + L        | 格式化代码，可以对当前文件和整个包目录使用                   |
| Alt + 7               | 显示当前类中的所有方法、全局常量，方法还包括形参和返回值     |
| Alt + F7              | 可以查看一个Java类、方法或变量的直接使用情况。               |
| ctrl + alt +b         | 查看接口的实现类                                             |
| ctrl + h              | 查看类或接口的继承关系                                       |
| F7                    | 在 Debug 模式下，进入下一步，如果当前行断点是一个方法，则进入当前方法体内，Step Into |
| F8                    | 在 Debug 模式下，进入下一步，如果当前行断点是一个方法，则不进入当前方法体内，Step Over |
| F9                    | 在 Debug 模式下，恢复程序运行，但是如果该断点下面代码还有断点则停在下一个断点上，Resume |

### 2．**设置eclipse风格快捷键**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/IDEA/wps228.jpg) 

 

常用快捷键：

| 提示补全                                     | Alt + /                  |
| -------------------------------------------- | ------------------------ |
| 单行注释                                     | ctrl + /                 |
| 多行注释                                     | ctrl + shift + /         |
| 向下复制一行                                 | Ctrl + alt + down        |
| 删除一行或选中行                             | Ctrl + d                 |
| 向下移动行                                   | Alt + down               |
| 向上移动行                                   | Alt + up                 |
| 万能纠错/生成返回值变量                      | Alt + enter（依然可用）  |
| 退回到前一个编辑的页面 (back)                | Alt + left               |
| 进入到下一个编辑的页面(针对于上条) (forward) | Alt + right              |
| 查看继承关系(type hierarchy)                 | F4                       |
| 格式化代码                                   | ctrl+shift+F             |
| 选中数行，整体往后移动                       | tab                      |
| 选中数行，整体往前移动                       | shift + tab              |
| 查看类的结构：类似于eclipse 的outline        | ctrl+o                   |
| 修改变量名与方法名                           | Alt + shift + r          |
| 大写转小写/ 小写转大写                       | Ctrl + shift + y/x       |
| 生成构造器/get/set/toString                  | Alt + insert（依然可用） |
| Shift+shift                                  | 万能搜索（依然可用）     |
| 查找/替换(当前)                              | Ctrl + f                 |
| 查找(全局)                                   | Ctrl + h                 |
| 查看类的继承结构图                           | Ctrl + ALT+ u            |
| 打开最近改的文件                             | Ctrl + E                 |
| 查找方法在哪里被调用                         | Ctrl + G                 |

 