# Spring

# Spring框架的江湖地位

职业生涯，离不开它

Spring框架基于IOC和AOP两大核心思想所做的技术实现，前无古人

Spring框架的体系非常庞大，Spring全家桶

 

# [了解]Spring介绍

l 什么是Spring

Spring在经典三层中的位置，各个层都有体现

![1576486441509](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486441509.png)

 

Spring是一个一站式/full-stack（全栈）的框架

不仅可以用Spring开发web层，也可以开发service层，还有dao层，归根揭底，它是一个对象的容器，它能够处理请求是因为这个容器中有能够处理请求的对象，能够操作数据库，是因为这个容器中有能够操作数据库的对象

 

根本上，就是一个对象的容器（Map）

l Spring的发展历程

了解讲义

l Spring的优势

了解讲义

l Spring的框架结构

![1576486464862](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486464862.png)

注意：Spring是一个模块化的框架，它的功能很强大，但是并不要求你完全都得用，图中每一个圆角矩形都代表一个jar，使用哪些模块引入哪些模块即可

# [理解]IOC的概念和作用

说明：

Spring所使用的两大思想IOC和AOP，都不是Spring提出的，之前更倾向于理论化，Spring框架把这两大思想在技术上做了非常好的实现。

l IOC的概念

IOC：Interverse of control （控制反转/反转控制）。原来我们在类中需要其他类时我们自己new对象，有了Spring之后，不需要我们自己new，框架已经准备好了对象，等待我们使用，我们只需要问框架要对象即可

我们丧失了一个权利（创建对象），得到了一个福利（不用管什么时候创建对象，以及对象什么时候销毁）

l IOC的作用

解决的是程序耦合问题

l 关于程序耦合(有一个认识即可)

耦合：耦合度，为了完成一定的功能，一个类调用了另外一个类，所产生的影响，影响大小代表耦合度的高低。

注意：耦合不能够绝对避免，耦合只能降低

“高内聚、低耦合”，麻烦自己，方便别人，你修改时候，别人对你的调用就不用变动

 

# [理解]自定义IOC实现程序解耦合

需求：开发dao层和service层模拟保存账户（Account），不具体操作数据库

l service层调用dao层存在耦合，解耦合分析过程

![1576486495348](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486495348.png)

l 需要实例化的类的全限定类名配置在xml中，便于工厂使用反射技术实例化对象

![1576486507775](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486507775.png)

l 使用工厂模式解耦合，分析并完成工厂类的两大任务

```java
package com.spring.factory;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.InputStream;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * 产生对象的工厂
 * 任务一：读取解析xml，根据配置信息实例化对象，放入map集合（对象容器），等待使用
 * 任务二：提供一个从map中获取对象的方法（根据id获取对象）
 */
public class BeanFactory {

    // map集合（对象容器，存储对象的）
    private static Map<String,Object> map = new HashMap<>();


    //任务一：读取解析xml，根据配置信息实例化对象，放入map集合（对象容器），等待使用
    static {
        // 读取解析xml
        InputStream inputStream = BeanFactory.class.getClassLoader().getResourceAsStream("beans.xml");
        SAXReader saxReader = new SAXReader();
        try {
            Document document = saxReader.read(inputStream);
            Element rootElement = document.getRootElement();
            List<Element> selectNodes = rootElement.selectNodes("//bean");
            if(selectNodes != null && selectNodes.size() > 0) {
                for (int i = 0; i < selectNodes.size(); i++) {
                    Element element =  selectNodes.get(i);
                    String id = element.attributeValue("id");
                    String clazz = element.attributeValue("class");
                    Class aClass = Class.forName(clazz);
                    Object o = aClass.newInstance();
                    // 把反射生成的对象放入对象容器待用
                    map.put(id,o);
                }
            }
        } catch (DocumentException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        }
    }



    //任务二：提供一个从map中获取对象的方法（根据id获取对象）
    public static Object getBean(String id) {
        return map.get(id);
    }

}
```





# [掌握]使用SpringIOC实现程序解耦合

l 从IOC容器获取对象相比原有方式对比

![1576486534430](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486534430.png)

 

l 引入jar坐标 

```xml
<!--引入spring容器坐标-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
```



l service层和dao层的业务代码完善

l 实例化service和dao代码，把它们的信息写在spring配置文件中

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountDao" class="com.spring.dao.AccountDaoImpl"/>
    <bean id="accountService" class="com.spring.service.AccountServiceImpl"/>
</beans>
```



l 启动容器，通过容器获取对象

```java
import com.spring.dao.AccountDao;
import com.spring.service.AccountService;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MainTest {

    /**
     * 测试用例：使用SpringIOC获取对象
     */
    @Test
    public void testSpringIoc() {
        // 启动对象容器
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        // getBean
        AccountService accountService = (AccountService) applicationContext.getBean("accountService");
        AccountDao accountDao = (AccountDao) applicationContext.getBean("accountDao");
        //AccountService accountService1 = applicationContext.getBean(AccountServiceImpl.class);
        //AccountService accountService1 = applicationContext.getBean(AccountService.class);
        //applicationContext.getBean("accountService",AccountService.class);

        System.out.println(accountService);
        System.out.println(accountDao);
    }
}
```





# [掌握]SpringIOC的一些细节知识

## ApplicationContext和BeanFactory

ApplicationContext在启动SpringIOC容器的时候，会立即创建对象放入map中（***\*不存在影响性能的问题\****）

BeanFactory在启动SpringIOC容器的时候，不会立即创建对象，getBean使用对象的时候，Spring才去创建对象（类似于懒加载）

## Bean标签

l id：标识符，定位对象的。id不能重复，没有mybatis中namespace之说了

class ：所创建对象的全限定类名，类似于：com.spring.dao.AccountDaoImpl，不能配置为接口，因为接口不能实例化 

l scope ：对象的作用范围，单例和多例，单例：内存中这个类只有这一个对象，创建之后一直使用这一个；多例：当你getBean的时候，每getBean一次都给你创建一个新的对象

singleton：单例/单个的意思，默认bean的scope都是单例

prototype：多例的意思，每getBean一次就new一个新的对象返回

request：在web应用的一个request请求内有效，下个请求内就重新new一个对象

session：在web应用的一个session会话内有效，下个会话重新new一个对象

l 生命周期属性

bean即对象，对象就有生命周期，生命周期就有开始和结束，有时候希望在对象创建之后立马做点啥，或者销毁之前做事情	

![1576486574789](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486574789.png)

 

l 对象创建的三种方式

通过配置class全限定类名框架底层使用反射技术来创建对象(经常使用，推荐)

![1576486587009](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486587009.png)

静态方法自己new对象，然后加入到Spring的对象容器中管理）

| ![img](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/wpsZf4iY4.jpg) |
| ------------------------------------------------------------ |
|                                                              |

 ```java
package com.spring.factory;

import com.spring.service.AccountService;
import com.spring.service.AccountServiceImpl;

public class StaticFactory {

    public static AccountService createAccountService() {
        return new AccountServiceImpl();
    }
}
 ```



动态实例化方法（自己new对象，然后加入到Spring的对象容器中管理）（需要首先定义工厂的bean，然后通过工厂bean对象调用里面的方法返回具体对象）

| ![img](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/wpsHrlXeF.jpg) |
| ------------------------------------------------------------ |
|                                                              |

 ```java
package com.spring.factory;

import com.spring.service.AccountService;
import com.spring.service.AccountServiceImpl;

public class InstanceFactory {

    public AccountService createAccountService() {
        return new AccountServiceImpl();
    }
}
 ```



## 依赖注入（DI）

DI: dependence Inject （依赖注入），注入对象所依赖的对象

### Set注入(也叫设值注入，这是常用和推荐的用法)

| ![img](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/wpshChfOE.jpg) |
| ------------------------------------------------------------ |
| ![img](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/wpsScXrx1.jpg) |

 

### 构造函数注入

![1576486619009](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486619009.png)

 

### 复杂对象注入

```xml
<property name="myStrs">
    <set>
        <value>mySet1</value>
        <value>mySet2</value>
        <value>mySet3</value>
    </set>
</property>

<property name="myList">
    <array>
        <value>myStr1</value>
        <value>myStr2</value>
        <value>myStr3</value>
    </array>
</property>

<property name="mySet">
    <list>
        <value>myList1</value>
        <value>myList2</value>
        <value>myList3</value>
    </list>
</property>

<property name="myMap">
    <props>
        <prop key="myProp1">propvalue1</prop>
        <prop key="myProp2">propvalue2</prop>
        <prop key="myProp3">propvalue3</prop>
    </props>

</property>

<property name="myProps">
    <map>
        <entry key="map1" value="mapvalue1"></entry>
        <entry key="map2" value="mapvalue2"></entry>
        <entry key="map3" value="mapvalue3"></entry>
    </map>
</property>
```

```java
private String[] myStrs;
private List<String> myList;
private Set<String> mySet;
private Map<String,String> myMap;
private Properties myProps;
setter方法…
```





# [了解]扩展知识

## Spring中使用BeanFactory获取对象

```java
@Test
public void testBeanFactory(){
    // 加载跟目录下的配置文件
    ResourcePatternResolver resolver = new PathMatchingResourcePatternResolver();
    Resource res = resolver.getResource("applicationContext.xml");
    // 创建Bean工厂类 注意 XmlBeanFactory已经过期!
    BeanFactory factory = new XmlBeanFactory(res);
    AccountDao accountDAO = factory.getBean("accountDao", AccountDao.class);
    AccountService accountService = factory.getBean("accountService", AccountService.class);
    System.out.println(accountDAO);
    System.out.println(accountService);
}
```



# count表的CRUD实现[掌握]

Dbutils使用：

QueryRunner对象的使用，提供了query和update（新增、删除和修改）接口

BeanHandler和BeanListHandler帮我们封装结果集数据

## 思路分析

l 从Maven坐标（jar）的角度分析

junit

mysql驱动

dbutils工具

c3p0/dbcp数据库连接池

![1576486642403](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486642403.png)

Apache提供的一个数据库连接池

spring-context（容器功能，帮我们管理对象以及注入对象）

l 从开发步骤的角度分析

pom文件引入坐标

pojo类

dao层开发

service层开发

使用Spring-context容器功能***\*管理涉及到的所有对象及其依赖注入关系\****

![1576486657718](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486657718.png)

每一个方块或者椭圆都是一个对象（需要定义bean），然后维护他们的注入关系

xml中配置如下 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--数据库连接池-->
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/spring?characterEncoding=utf-8"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>


    <!--queryrunner对象-->
    <bean id="queryRunner" class="org.apache.commons.dbutils.QueryRunner">
        <constructor-arg name="ds" ref="dataSource"/>
    </bean>

    <!--dao层对象-->
    <bean id="accountDao" class="com.spring.dao.AccountDaoImpl">
        <property name="queryRunner" ref="queryRunner"/>
    </bean>

    <!--service层对象-->
    <bean id="accountService" class="com.spring.service.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"/>
    </bean>
</beans>
```



l DBCP数据库连接池

![1576486676899](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486676899.png)

注意：所有的数据库连接池使用套路都一样，都是使用其中一个核心类，然后赋值必要的数据库连接四要素

# [掌握]SpringIOC的注解实现

原则：找xml中配置项和注解的对应

注意：使用注解不需要引入额外的jar包

l Bean标签功能的注解实现方式

| xml形式                        | 对应的注解形式                                               |
| ------------------------------ | ------------------------------------------------------------ |
| <bean>标签定义一个对象         | @Component("accountDao")bean的id属性内容直接配置在注解后面如果不配置，默认定义个这个bean的id为类的类名首字母小写另外，针对分层代码开发提供了@Componenet的三种别名@Controller、@Service、@Repository分别用于控制层类、服务层类、dao层类的bean定义这四个注解的用法完全一样，只是为了更清晰的区分而已 |
| <bean>标签的scope属性          | @Scope("prototype")，默认单例                                |
| <bean>标签的init-method属性    | @PostConstruct，注解加载方法上                               |
| <bean>标签的destory-method属性 | @PreDestory，注解加载方法上                                  |

 

l 依赖注入的注解实现方式

@AutoWired注解（Spring框架提供的，推荐使用）

按照类型注入

![1576486697145](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486697145.png)

n @Resource注解（JDK提供）

按照id注入

![1576486705999](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486705999.png)

使用Resource注解的name属性来指定注入对象的id

l 开启注解扫描开关

![1576486719178](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486719178.png)

 

# [掌握]Account表CRUD的半xml半注解方式实现

企业开发使用最多的方式

l xml配置文件依然存在，从配置文件启动容器

l 半xml：指的是第三方jar中的对象的定义配置在xml中

l 半注解：自己开发的类定义bean使用注解方式

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
">

    <!--
        配置注解扫描
        base-package：配置所扫描包路径
    -->
    <context:component-scan base-package="com.spring"/>

    <!--数据库连接池-->
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/spring?characterEncoding=utf-8"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>


    <!--queryrunner对象-->
    <bean id="queryRunner" class="org.apache.commons.dbutils.QueryRunner">
        <constructor-arg name="ds" ref="dataSource"/>
    </bean>
</beans>
```



# [掌握]Account表CRUD的纯注解方式实现

纯注解方式（不需要引入额外的jar包）

其实是对半xml半注解模式的进一步改造

**l** 重点改造第三方jar中对象的定义（注解实现）

**l** 容器初始化不是加载xml的方式了（纯注解不存在xml文件了）

![1576486770371](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486770371.png)



l @Configuration注解，表名当前类是一个配置类

@ComponentScan注解，替代<context:component-scan>

@ PropertySource,引入外部属性配置文件

@Import引入其他配置类

@Value对变量赋值，可以直接赋值，也可以使用${}读取资源配置文件中的信息

@Bean将方法返回对象加入SpringIOC容器 

```java
package com.spring.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import org.springframework.context.annotation.PropertySource;

@Configuration  // 标识当前类是Spring的一个配置类
@ComponentScan(value = {"com.spring"})
@PropertySource(value = {"db.properties"})  // @PropertySource注解用于加载属性配置文件
@Import(value = {JdbcConfig.class})         // @Import引入其他配置类
public class SpringConfig {
}
```

```java
package com.spring.config;

import org.apache.commons.dbcp2.BasicDataSource;
import org.apache.commons.dbutils.QueryRunner;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;

@Configuration
public class JdbcConfig {

    // @Value注解对变量赋值：1、可以直接赋值 2、可以使用${}获取属性文件配置项
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;



    /**
     * @Bean注解加载方法上
     * 它可以把方法的返回值（返回值是一个对象），把这个对象加入到spring的ioc容器中
     * @return
     */
    @Bean("dataSource")
    public DataSource createDataSource() {
        BasicDataSource basicDataSource = new BasicDataSource();
        basicDataSource.setDriverClassName(driver);
        basicDataSource.setUrl(url);
        basicDataSource.setUsername(username);
        basicDataSource.setPassword(password);
        return basicDataSource;
    }


    @Bean("queryRunner")
    public QueryRunner createQueryRunner(@Qualifier("dataSource") DataSource dataSource) {
        QueryRunner queryRunner = new QueryRunner(dataSource);
        return queryRunner;
    }
}
```



l启动容器测试

```java
@Test
public void testQueryAccountById() throws Exception {
    // 启动ioc容器（全注解模式）
    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(SpringConfig.class);
    AccountService accountService = (AccountService) applicationContext.getBean("accountService");
    Account account = accountService.queryAccountById(1);
    System.out.println(account);
}
```





注意：

使用Spring的时候，查找资源文件，咱们统一养成习惯加上classpath：

classpath：和classpath\*：的区别，classpath会去工程classes目录下查找（不包括jar），classpath\*会包括jar的所有的classes下去查找

 

# [掌握]Spring对Junit的支持

l 引入spring-test的jar包

```xml
<!--Spring对junit测试的支持-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
```



l Spring对junit支持所带来的好处

1、配置之后不需要我们去启动容器

2、测试哪个对象直接可以注入（使用注解）

纯注解启动的形式

```java
// 使用Spring自定义的junit运行器替换它默认的运行器
@RunWith(SpringJUnit4ClassRunner.class)
// 指定配置文件或者配置类
@ContextConfiguration(classes = {SpringConfig.class})
public class MainTest {

    @Autowired
    private AccountService accountService;

    @Test
    public void testQueryAccountById() throws Exception {
        Account account = accountService.queryAccountById(1);
        System.out.println(account);
    }
}
```





读取xml配置文件启动的形式

![1576486797031](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486797031.png)

最终，让我们非常方便的注入对象，测试对象

# [理解]AOP概述

## 什么是AOP

AOP：Aspect Oriented Programming（面向切面编程/面向方面编程）

先从OOP说起（面向对象编程）

三大特征：封装、继承和多态

Horse类、Cat类、Dog类，都有eat()、run()方法，抽取父类（Animal类），在父类中写上重复的代码，这样避免了代码重复问题

![1576486808880](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486808880.png)

大多数情况下OOP编程都能够满足我们的需求，但是事情不是简单的，在一些场合使用OOP思想来解决代码重复问题，已经不行了

![1576486819773](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486819773.png)

横切代码存在什么问题？

l 横切代码往往存在于多个方法中，是重复的

l 横切代码和业务代码混杂在一起，造成业务代码非常冗余，对于维护和编程都不是好现象

AOP别出心裁的提出了横向抽取机制，把业务代码和横切代码分离，将业务代码和横切代码分离容易，但是最终终究要横切代码应用于业务代码，这是比较难的，也是AOP重点要解决的问题
即，不改变原有业务逻辑的基础上增强你的横切逻辑

AOP的底层实现原理：动态代理技术；

 ![1576486834861](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486834861.png)

 

## 为什么叫AOP（面向切面编程）

我们要做的事就是在不改变原有业务逻辑的基础上增强我们的横切逻辑，业务逻辑不能改，我们只能操作横切逻辑（切的意思所在，我们只能面向横切逻辑），AOP要增强的横切逻辑往往影响的不是一个方法，往往很多方法，每一个方法如同一个散点，很多散点组成面（这是面向切面编程中“面”的意思所在），这就是为什么叫做面向切面编程

 

 

## AOP的实现方式

Spring的AOP的实现方式就是动态代理技术

## AOP的作用及优势

l 作用：程序运行期间，在不改变原有业务逻辑的情况下进行方法增强 

l 优势：减少重复代码，提高开发效率，业务逻辑和增强的横切逻辑分离便于维护

# [理解]AOP的具体应用

模拟转账

l aaa账户向bbb账户转钱

l aaa账户减钱，bbb账户加钱（转账业务）

查找到aaa、bbb账户（根据账户名查找账户）

进行转账计算

更新aaa、bbb账户信息到数据库（根据id更新账户）

 

 

案例：模拟转账（并且模拟转账异常）

l 汇款人账户减少一定的金额

l 收款人账户增加一定的金额

l 计算之后，更新数据库

问题：模拟转账异常（人为制造异常，在两次update之间造了异常）

```java
/**
 * 转账思路分析：
 * 1、根据账户名查询出汇款人账户和收款人账户
 * 2、计算转账金额：汇款人—money，收款人+money
 * 3、把上述计算后的数据更新到数据库
 * @param fromName 汇款人
 * @param toName   收款人
 * @param money    转账金额
 * @throws Exception
 */
@Override
public void transfer(String fromName, String toName, Float money) throws Exception {
    Account fromAccount = accountDao.queryAccountByName(fromName);
    Account toAccount = accountDao.queryAccountByName(toName);

    // 计算转账
    fromAccount.setMoney(fromAccount.getMoney() – money);
    toAccount.setMoney(toAccount.getMoney() + money);

    // 更新到数据库
    accountDao.updateAccountById(fromAccount);
    // 人为制造的异常
    int c = 1/0;
    accountDao.updateAccountById(toAccount);
}
```



问题分析：

l 两次update走了两个事务，归根结底是两次update使用了两个不同的连接

l 当前事务在dao层，因为你调用了两次dao，两次又走了不同的事务

 

解决方案：

l 两次update使用同一个连接（两次update有一个关系：就是在一个线程中，这样把连接绑定到当前线程）

l 事务控制在service层

 

创建ConnectionUtil类

```java
package com.spring.utils;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

public class ConnectionUtil {

    // 存放和当前线程绑定的连接
    private ThreadLocal<Connection> threadLocal = new ThreadLocal<>();

    private DataSource dataSource;

    /**
     * 返回当前线程绑定的连接对象
     * @return
     */
    public Connection getCurrentThreadConn() throws SQLException {
        Connection connection = threadLocal.get();
        if(connection == null) {
            connection = dataSource.getConnection();
            threadLocal.set(connection);
        }
        return connection;
    }


    /**
     * 解除线程和连接的绑定关系
     */
    public void remove() {
        threadLocal.remove();
    }
}
```

```java
package com.spring.dao;

import com.spring.pojo.Account;
import com.spring.utils.ConnectionUtil;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Repository;

@Repository("accountDao")
public class AccountDaoImpl implements AccountDao {

    @Autowired
    @Qualifier("queryRunner")
    private QueryRunner queryRunner;

    private ConnectionUtil connectionUtil;

    @Override
    public Account queryAccountByName(String name) throws Exception {
        String sql = "select id,name,money from account where name=?";
        return queryRunner.query(connectionUtil.getCurrentThreadConn(),sql,new BeanHandler<Account>(Account.class),name);
    }

    @Override
    public int updateAccountById(Account account) throws Exception {
        String sql = "update account set name=?,money=? where id=?";
        return queryRunner.update(connectionUtil.getCurrentThreadConn(),sql,account.getName(),account.getMoney(),account.getId());
    }
}
```





l 事务应该加载service业务层上，而且应该由我们自己去控制（关闭事务自动提交）

```java
package com.spring.utils;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Component;

/**
 * 事务管理器：开启事务、提交事务、回滚事务等
 */
@Component("transactionManager")
public class TransactionManager {

    @Autowired
    @Qualifier("connectionUtil")
    private ConnectionUtil connectionUtil;


    /**
     * 开启事务（就是关闭连接的事务自动提交功能）
     */
    public void beginTransaction() throws Exception {
        connectionUtil.getCurrentThreadConn().setAutoCommit(false);
    }


    /**
     * 提交事务
     * @throws Exception
     */
    public void commit() throws Exception{
        connectionUtil.getCurrentThreadConn().commit();
    }


    /**
     * 回滚事务
     * @throws Exception
     */
    public void rollback() throws Exception{
        connectionUtil.getCurrentThreadConn().rollback();
    }


    /**
     * 释放资源
     * @throws Exception
     */
    public void release() throws Exception{
        // 关闭连接
        connectionUtil.getCurrentThreadConn().close();
        // 解除绑定关系
        connectionUtil.remove();
    }
}
```

```java
@Override
public void transfer(String fromName, String toName, Float money) throws Exception {
    try{
        // 开启事务（把事务的自动提交关闭即可）
        transactionManager.beginTransaction();

        // 业务逻辑代码

        Account fromAccount = accountDao.queryAccountByName(fromName);
        Account toAccount = accountDao.queryAccountByName(toName);

        // 计算转账
        fromAccount.setMoney(fromAccount.getMoney() - money);
        toAccount.setMoney(toAccount.getMoney() + money);

        // 更新到数据库
        accountDao.updateAccountById(fromAccount);
        // 人为制造的异常
        int c = 1/0;
        accountDao.updateAccountById(toAccount);

        // 提交事务
        transactionManager.commit();
    }catch (Exception e) {
        e.printStackTrace();
        // 回滚事务
        transactionManager.rollback();
    }finally {
        // 释放资源
        transactionManager.release();
    }

}
```



我们不可能在每一个方法中添加try…catch…finally控制，因此引入动态代理技术来在不改变原有业务逻辑代码的基础上做逻辑增强

 ```java
package com.spring.factory;

import com.spring.utils.TransactionManager;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Component;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/**
 * 代理对象工厂
 */
@Component("proxyFactory")
public class ProxyFactory {

    @Autowired
    @Qualifier("transactionManager")
    private TransactionManager transactionManager;

    public Object getProxy(Object object) {
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), object.getClass().getInterfaces(), new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                Object result = null;

                try{
                    // 开启事务（把事务的自动提交关闭即可）
                    transactionManager.beginTransaction();
                    // 业务逻辑代码
                    result = method.invoke(object,args);
                    // 提交事务
                    transactionManager.commit();
                }catch (Exception e) {
                    e.printStackTrace();
                    // 回滚事务
                    transactionManager.rollback();
                }finally {
                    // 释放资源
                    transactionManager.release();
                }
                return result;
            }
        });
    }
}
 ```



# [掌握]Spring中的AOP使用

## AOP相关术语

l Joinpoint（连接点） （程序中所有方法的方法前、方法后、异常时等时机都是连接点）

横切程序执行的特定位置，比如类开始初始化前，类初始化之后，类中某个方法调用前、调用后，方法抛出异常后（时机）等，这些代码中的特定点就称为“连接点”。

Spring仅支持方法的连接点，即仅能在方法调用前、方法调用后、方法抛出异常时以及方法调用前后这些程序执行点织入增强。

我们知道黑客攻击系统需要找到突破口，没有突破口就无法进行攻击，从这一角度上来说，AOP是一个黑客（因为它要向目前类中嵌入额外的代码逻辑），连接点就是AOP向目标类打入楔子的***\*候选点\****。

l Pointcut（切入点）（定位你感兴趣的方法）

一个类中可以有很多个方法，每个方法又有多个Joinpoint，在这么多个方法中，如何定位到自己感兴趣的方法呢？靠的是切点

注意：切点只定位到某个方法上，所以如果希望定位到具体连接点上，还需要提供方位信息 

比如：如果把一个方法理解成数据表中的一条记录的话，那么切入点就好比你select语句的where条件，就可以定位到你感兴趣的方法

l Advice（通知/增强）

增强的第一层意思就是你的横切逻辑代码（增强逻辑代码）

在Spring中，增强除了用于描述横切逻辑外，包含一层意思就是横切逻辑执行的方位信息。

刚刚说了切点只能定位到方法，在进一步使用方位信息就可以定位到我们感兴趣的连接点了（方法调用前、方法调用后还是方法抛出异常时等）。  

l Target（目标对象）即委托对象

增强逻辑的织入目标类。比如未添加任何事务控制的AccountServiceImplNoTcf类 

l Weaving（织入） 形象的描述了增强横切逻辑的整个过程

织入是将增强逻辑/横切逻辑添加到目标类具体连接点上的过程，AOP像一台织布机，将目标类、增强或者引介通过AOP（其实就是动态代理技术）这台织布机天衣无缝地编织到一起。

Spring采用动态代理织入。 

l Proxy（代理）代理对象

一个类被AOP织入增强后，就产出了一个结果类，它是融合了原类和增强逻辑的代理类。 

l Aspect（切面）

切面由切点和增强组成。

切面=切点+增强

​	=切点+方位信息+横切逻辑

​	=连接点+横切逻辑

最终切面完成：把横切逻辑织入到哪些方法的方法前/后等

本质：把横切逻辑增强到连接点（切点和方位信息都是为了确定连接点）上

## Spring关于JDK/CGLIB动态代理的选择

Spring发现涉及到接口那就使用JDK动态代理，如果不涉及接口就使用CGLIB动态代理

AOP:日志、性能监控、事务、权限控制

## 基于XML的AOP配置

l 需求：在Service层代码的不同方法的不同连接点JoinPoint织入日志

把Account表的service层进行crud模拟（dao层就不需要了）

l 引入POM坐标

```xml
<!--使用Spring的AOP功能-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
<!--SpringAop功能依赖的jar-->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.8.9</version>
</dependency>
```



l applicationContext.xml配置

l 切入点表达式说明（切入点表达式语法使用的是AspectJ框架的表达式语法）

![img](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/wpsXp2qJ4.jpg) 

l XML配置及四种常用Advice通知类型（即四种方位）

aop:before ：配置前置通知/增强，指定增强的方法在切入点方法之前执行 

aop:after-returning：配置后置通知，切入点方法正常执行之后，相当于图中位置

此处等同于try{   …}catch(){}finally{}，红色点标注的时机

aop:after-throwing：配置异常通知，切入点方法执行产生异常后执行，它和后置通知只能执行一个，等同于catch中的时机

aop:after：配置最终通知，无论切入点方法执行时是否有异常，它都会在其后面执行，等同于finally中的执行时机

l 环绕通知：它是spring为我们提供的一种可以在代码中手动控制增强方法何时执行的方式，灵活度比较高，设置可以控制原业务逻辑是否执行,其他四种通知类型的原有业务逻辑，肯定执行！

注意：通常情况下，环绕通知都是独立使用的，不要和上面的四种通知类型混合使用

 ```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
">

    <context:component-scan base-package="com.spring"/>



    <!--配置AOP内容都放置在aop:config标签下-->
    <!--横切逻辑bean-->
    <bean id="logUtil" class="com.spring.utils.LogUtil"/>
    <aop:config>
        <!--aop主要就是配置切面，所以aop:aspect
            切面=切点+方位信息+横切逻辑
            ref：指向横切逻辑
            <aop:before>等标签已经表明方位信息
            切点：使用pointcut进行定义
        -->
        <aop:aspect id="logAspect" ref="logUtil">
            <!--aop:pointcut单独配置切入点表达式，供引用-->
            <aop:pointcut id="pt1" expression="execution(* com.spring.service.*.*(..))"/>
            <!--<aop:before method="printBeforeMethod" pointcut-ref="pt1"/>
            <aop:after-returning method="printAfterReturn" pointcut-ref="pt1"/>
            <aop:after-throwing method="printAfterThrowing" pointcut-ref="pt1"/>
            <aop:after method="printAfterMethod" pointcut-ref="pt1"/>-->

            <aop:around method="printRound" pointcut-ref="pt1"/>
        </aop:aspect>
    </aop:config>
</beans>
 ```



## 基于注解的AOP配置

l 半xml半注解形式

在xml配置文件中开启AOP注解开关

![1576486895440](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486895440.png)

在横切逻辑类上配置

```java
package com.spring.utils;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

@Component
@Aspect
public class LogUtil {

    @Pointcut("execution(* com.spring.service.*.*(..))")
    public void pt1() {

    }

  /*  *//**
     * 方法执行之前打印
     *//*
    @Before("pt1()")
    public void printBeforeMethod() {
        System.out.println("方法执行之前打印");
    }


    *//**
     * 方法结束时打印
     *//*
    @After("pt1()")
    public void printAfterMethod() {
        System.out.println("方法执行之后（结束）打印");
    }


    *//**
     * 方法异常时打印
     *//*
    @AfterThrowing("pt1()")
    public void printAfterThrowing() {
        System.out.println("方法异常时打印");
    }


    *//**
     * 方法正常执行（没有抛出异常）时打印
     *//*
    @AfterReturning("pt1()")
    public void printAfterReturn() {
        System.out.println("方法正常执行（返回）打印");
    }*/


    /**
     * 环绕通知，灵活度比较高，可以手动控制在业务逻辑之前、之后、异常时等连接点执行逻辑
     * 也可以控制原有业务逻辑是否执行，通过直接在参数中声明形参ProceedingJoinPoint即可，类似于动态代理中的method.invoke()
     * @param proceedingJoinPoint
     * @return
     */
    @Around("pt1()")
    public Object printRound(ProceedingJoinPoint proceedingJoinPoint) {

        Object result = null;
        try {
            System.out.println("环绕通知的之前执行");
            result = proceedingJoinPoint.proceed();
            System.out.println("环绕通知的正常执行时执行");
        } catch (Throwable throwable) {
            throwable.printStackTrace();
            System.out.println("环绕通知的异常执行时执行");
        }
        System.out.println("环绕通知方法结束时执行");
        return result;
    }
}
```



l 全注解模式下注解配置开关

```java
@Configuration //标识当前类是spring的一个配置类
@ConmponentScan(value={"com.spring"})
@EnableAspectJAutoProxy
public class SpringConfig {
}
```





## 学习spring中的AOP时要明确的事

l AOP的使用场景

AOP的应用场景往往是受限的，它一般只适合于那些具有横切逻辑的应用场合：如性能监测、访问控制、事务管理以及日志记录等。不过，这丝毫不影响AOP作为一种新的软件开发思想在软件开发领域所占有的地位。

l 开发阶段（我们完成） 

编写核心业务代码

大部分程序员来做，要求熟悉业务需求。 

抽取公用代码制作成通知，进行AOP配置

一般由专门的AOP编程人员来做

l 运行阶段（Spring框架完成） 

Spring框架监	控切入点方法的执行。一旦监控到切入点方法被运行，Spring框架使用代理机制，动态创建目标对象的代理对象，根据通知类别，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行。

# [应用]JdbcTemplate数据库操作工具使用

Spring框架提供的一个数据库操作工具，使用方法和Dbutils非常相似

 

| Dbutils                  | JdbcTemplate                                          |
| ------------------------ | ----------------------------------------------------- |
| 核心对象QueryRunner      | 核心对象JdbcTemplate                                  |
| update接口（增、删、改） | update接口（增、删、改），操作过程和方式和dbutils一样 |
| query接口查询            | queryForObject（单个值/单个对象）                     |
|                          | query接口（返回集合）                                 |

Dbutils返回单个对象使用BeanHandler封装数据，返回list集合使用BeanListHandler封装数据。BeanHandler和BeanListHandler都是ResultsetHandler接口的实现类

 

在JdbcTemplate中，封装数据也提供了接口RowMapper，但是没有给你实现类，只能我们自己去实现这个接口，告诉工具结果集封装规则

 

​		 

## JdbcTemplate入门使用

l 需求：使用JdbcTemplate工具完成Account表的Crud

l 操作步骤

引入jar包坐标

```xml
<!--jdbcTemplate使用的jar-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
<!--事务相关的jar，往往和jdbc一起引入-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
```

l CRUD程序

```java
import com.spring.pojo.Account;
import org.junit.Before;
import org.junit.Test;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;

public class JdbcTemplateTest {

    private JdbcTemplate jdbcTemplate;

    @Before
    public void before() {
        // 数据库连接池（使用Spring内置的数据库连接池）
        DriverManagerDataSource driverManagerDataSource = new DriverManagerDataSource();
        driverManagerDataSource.setDriverClassName("com.mysql.jdbc.Driver");
        driverManagerDataSource.setUrl("jdbc:mysql://localhost:3306/spring?characterEncoding=utf-8");
        driverManagerDataSource.setUsername("root");
        driverManagerDataSource.setPassword("root");


        // 实例化JdbcTemplate工具的核心对象
        jdbcTemplate = new JdbcTemplate(driverManagerDataSource);
        // jdbcTemplate.setDataSource(driverManagerDataSource);
    }



    /**
     * jdbcTemplate工具之insert操作
     */
    @Test
    public void testSaveAccount() {
        String sql = "insert into account(name,money) values(?,?)";
        int  count = jdbcTemplate.update(sql,"柳岩",100f);
    }

    /**
     * jdbcTemplate工具之update操作
     */
    @Test
    public void testUpdateAccountById() {
        String sql = "update account set name=?,money=? where id=?";
        int count = jdbcTemplate.update(sql,"唐嫣",1000f,4);
        System.out.println(count);
    }


    /**
     * jdbcTemplate工具之delete操作
     */
    @Test
    public void testDeleteAccountById() {
        String sql = "delete from account where id=?";
        int count = jdbcTemplate.update(sql,4);
        System.out.println(count);
    }


    /**
     * jdbcTemplate工具之查询单个值（总记录数）
     */
    @Test
    public void testQueryAccountCount() {
        String sql = "select count(id) from account";
        /*
          返回单个值使用queryForObject接口
          param1：sql语句
          param2：返回值的数据类型
        */
        long count = jdbcTemplate.queryForObject(sql,long.class);
        System.out.println("account表总记录数：" + count);
    }


    /**
     * jdbcTemplate工具之查询单个值（字符串类型）（使用queryForObject接口）
     */
    @Test
    public void testQueryAccountNameById() {
        String sql = "select name from account where id=?";
        String name = jdbcTemplate.queryForObject(sql,String.class,1);
        System.out.println(name);
    }

    /**
     * jdbcTemplate工具之查询单个对象（使用queryForObject接口）
     */
    @Test
    public void testQueryAccountById() {
        String sql = "select id,name,money from account where id=?";
        Account account = jdbcTemplate.queryForObject(sql, new AccountRowMapper(), 1);

        System.out.println(account);
    }


    /**
     * jdbcTemplate工具之查询返回对象集合（使用query接口）
     */
    @Test
    public void testQueryAccountList() {
        String sql = "select id,name,money from account";
        List<Account> list = jdbcTemplate.query(sql, new AccountRowMapper());
        if(list != null && list.size() > 0) {
            for (int i = 0; i < list.size(); i++) {
                Account account =  list.get(i);
                System.out.println(account);
            }
        }
    }

    /**
     * AccountRowMapper内部类是对RowMapper接口的实现
     * 在查询单个对象或者对象结合的时候封装数据使用
     */
    public class AccountRowMapper implements RowMapper<Account>{
        @Override
        public Account mapRow(ResultSet rs, int rowNum) throws SQLException {
            Account account = new Account();
            account.setId(rs.getInt("id"));
            account.setName(rs.getString("name"));
            account.setMoney(rs.getFloat("money"));
            return account;
        }
    }
}
```





## JdbcTemplate开发Dao层的两种方式

l 需求：开发service层和dao层，service层调用dao层，在dao层中使用JdbcTemplate实现Account表的Crud（半xml半注解形式实现）

## 方式一：定义JdbcTemplate为普通Bean

普通玩法：和Dbutils一样，在Spring配置文件中定义JdbcTmeplate的bean对象

```xml
<!--JdbcTemplate工具-->
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

dao层使用JdbcTmeplate对象，直接注入使用

```java
package com.spring.dao;

import com.spring.pojo.Account;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.stereotype.Repository;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;

@Repository
public class AccountDaoImpl implements AccountDao {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public void saveAccount(Account account) throws Exception {
        String sql = "insert into account(name,money) values (?,?)";
        jdbcTemplate.update(sql,account.getName(),account.getMoney());
    }

    @Override
    public void updateAccountById(Account account) throws Exception {
        String sql = "update account set name=?,money=? where id=?";
        jdbcTemplate.update(sql,account.getName(),account.getMoney(),account.getId());
    }

    @Override
    public void deleteAccountById(Integer id) throws Exception {
        String sql = "delete from account where id=?";
        jdbcTemplate.update(sql,id);
    }

    @Override
    public Account queryAccountById(Integer id) throws Exception {
        String sql = "select id,name,money from account where id=?";
        return jdbcTemplate.queryForObject(sql,new AccountRowMapper(),id);
    }

    @Override
    public List<Account> queryAccountList() throws Exception {
        String sql = "select id,name,money from account";
        return jdbcTemplate.query(sql,new AccountRowMapper());
    }


    public class AccountRowMapper implements RowMapper<Account>{
        @Override
        public Account mapRow(ResultSet rs, int rowNum) throws SQLException {
            Account account = new Account();
            account.setId(rs.getInt("id"));
            account.setName(rs.getString("name"));
            account.setMoney(rs.getFloat("money"));
            return account;
        }
    }
}
```



 

## 方式二：继承JdbcDaoSupport类

玩法二和玩法一的区别在于：dao层实现类中jdbcTemplate对象的获取方式

Dao层实现类直接继承JdbcDaoSupport父类，从父类中获取JdbcTemplate对象

 

注意：推荐使用方式一，继承JdbcDaoSupport类的方式只支持xml配置，不支持注解方式

必须在xml中定义dao实现类对象（不能直接在dao实现类上添加@Repository注解）

```xml
 <!--继承JdbcDaoSupport的方式-->
    <bean id="accountDao" class="com.spring.dao.AccountDaoImpl">
        <property name="dataSource" ref="dataSource"/>
    </bean>
```



dao实现类中继承JdbcDaoSupport父类

![1576486942968](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486942968.png)

 

 

## Spring内置数据源

![1576486954802](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486954802.png)

 

# [理解掌握]Spring的声明式事务控制

l 关于编程式事务和声明式事务

编程式事务：在业务代码中添加事务控制代码，这样的事务控制机制，叫做编程式事务

声明式事务：通过xml配置或者注解方式就达到事务控制的目的，叫做声明式事务

## 关于事务

l 事务基本特性（ACID，是针对单个事务的一个完美状态）

原子性：一个事务内的操作要成功都成功，要失败都失败。比如转账案例，转账和到账要么同时成功要么同时失败。

一致性：一致性和原子性描述的是同一件事，只不过角度不一样。原子性是从一个事务内的操作的角度来说的，要成都成要失败都失败。一致性是从数据的角度来说的，比如转账，转账的初始数据状态（1000,1000），现在转100块，转账完成后，对外来说，数据状态要么（900,1100），要么是（1000,1000），不能够出现（900，1000）等中间状态

隔离性：比如事务1给员工涨工资2000元，但是事务1尚未提交事务，事务2查询工资，发现工资涨了2000块。这就是脏读（读到了未提交的数据）

持久性：事务一旦提交即生效，即使数据库服务器宕机，那么重启之后数据也应该是事务提交之后的状态，不会是之前的状态

l 事务并发问题

脏读

场景

财务人员发起事务1，给员工A涨了1w块钱，但是尚未提交事务。此时员工A发起了事务2查询工资，发现涨了1w块钱。

幻读（针对insert和delete操作）

场景

l 事务1查询所有工资为1w的员工的总数，查出来10个，此时事务尚未关闭。

l 事务2，由财务人员发起，新来两个员工，工资也是1w，向数据表中插入了两条数据，并且提交事务

l 事务1再次查询工资为1w的员工个数，发现有12个。见鬼了

不可重复读（针对update操作）

场景

l 员工A发起事务1，查询工资，工资为1w块钱，此时事务尚未关闭

l 财务人员发起了事务2给员工A涨了2000块钱，并且提交事务

l 员工A通过事务1再次查询工资，发现工资为1.2w，原来的1w那个数据读不到了，叫做不可重复读。

l 事务隔离级别（解决是事务并发问题的）

读未提交（极端）：Read Uncommited，读到了未提交的数据，什么并发问题也解决不了。不要采取这种方案

读已提交：Read Committed：解决脏读问题，解决不了幻读和不可重复读的问题（因为幻读和不可重复读问题的造成，本身就是已提交事务造成的）

可重复读：Read Repeatable，解决脏读和不可重复读的问题。解决不了幻读问题，因为可重复读针对的是update操作，在可重复读机制下，会对要update的语句进行加锁，加锁状态下其他事务无法修改。但是其他事务可以对表新增和删除

串行化（极端）：Serializable，事务一个个来，是最安全的隔离机制，但是效率较低，比如ATM机。

默认，数据库的默认，mysql默认的隔离机制是可重复读，Oracle的默认隔离机制是读已提交

l 事务传播行为

事务往往在service层进行控制，如果出现service层方法A调用了另外一个service层方法B，A和B方法本身都已经被添加了事务控制，那么A调用B的时候，就需要进行事务的一些协商，这就叫做事务的传播行为。

A调用B，我们站在B的角度来观察来定义事务的传播行为

![1576486977541](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486977541.png)

 

## Spring声明式事务配置

pom.xml

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.8.9</version>
</dependency>
```





Spring包容性很强，对dao层很多技术都会提供支持

jdbc：connection.commit()

mybatis：sqlSession.commit()

hibernate：session.commit();

Spring针对上述众多的事务调用方式，为了一统江湖，定义顶层接口（开启事务、提交事务、回滚事务等）

针对不同的技术体系，封装不同的事务管理实现类

 ![1576486989875](https://github.com/lagouedu/Java_L2_2019.12.23/blob/master/04_其他/img-folder/Spring/1576486989875.png)

 



事务管理器类，就是关于事务的横切逻辑，我们使用jdbc/jdbcTemplate/mybatis使用的是Spring提供的DataSourceTransactionMannager事务管理类，这个类就如同自定义AOP中开发的TransactionMannager类

 

事务控制本质：将横切逻辑（DataSourceTransactionMannager，此处的横切逻辑就是事务的控制逻辑）增强到感兴趣的方法上，进行事务控制

## 基于注解

l 半注解半xml形式 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd
">

    <context:component-scan base-package="com.spring"/>

    <context:property-placeholder location="classpath:db.properties"/>


    <!--使用Spring内置的数据库连接池，该连接池在Spring-jdbc这个jar中-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!--定义jdbcTemplate对象-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"/>
    </bean>


    <!--Spring的声明式事务是基于AOP技术实现
        横切逻辑单独定义为一个bean
        事务管理器底层还需要数据库连接池的支持
    -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>


    <tx:advice id="txAdvice" transaction-manager="transactionManager">

        <!--tx:attributes允许我们对事务细节进行配置-->
        <tx:attributes>
            <!--CRUD中只有查询是只读事务，其他都是非只读事务
                所以在这里可以首先配置所有方法都是非只读事务
                然后单独配置查询方法为只读事务，覆盖上面的配置
                这样，简化配置量
                read-only：是否只读
                propagation：事务传播行为
                isolation：事务隔离级别，默认defalut
                timeout：超时时间，-1没有限制，单位秒
                rollback-for：出现什么异常的时候回滚事务，往往不需要配置，有异常就回滚
                no-rollback-for：出现什么异常的时候不回滚事务，往往不需要配置，有异常就回滚
            -->
            <tx:method name="*" read-only="false" propagation="REQUIRED" isolation="DEFAULT" timeout="-1" />
            <!--查询操作为只读事务即可，事务传播行为为SUPPORTS，有事务就用，没有就不用-->
            <tx:method name="query*" read-only="true" propagation="SUPPORTS"/>
        </tx:attributes>
    </tx:advice>


    <aop:config>
        <!--Spring声明式事务使用aop:advisor替代aop中的aop:aspect
            至于方位信息不需要指定了（对于事务来说）
        -->
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.spring.service.*.*(..))"/>
    </aop:config>
</beans>
```



l 全注解形式开启开关

```java
@Configuration //标识当前类是spring的一个配置类
@PropertySource({"classpath:db.properties"})
@EnableTransactionManagement
public class SpringConfig {
}
```





 