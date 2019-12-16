# **Mybatis**

# **[了解]认识框架**

## **什么是框架**

l 框架说白了就是一个加载，比如表演节目，舞台已经搭建，表演节目看你需求

l 框架是一个半成品

l 对于java语言来说，框架就是别人代码的封装，拿来主义，在框架的基础上我们进一步开发。复用性/重用性

## **框架要解决的问题**

​	框架要解决的是技术整合问题。现在软件开发环境和软件规模非常大，我们不可能所有系统代码都从0开始敲，需要优秀的框架把基础技术都整合完了，我们在它的基础上进一步开发。

​	提高性能、安全性、易维护、易扩展。最终提高团队开发效率

## **什么时候使用框架**

​	企业级大型项目开发（避免大炮打蚊子）

## **怎么用框架**

Java框架共性：

l 一堆jar包，引入工程

l 对框架运行细节进行定制，往往通过xml配置文件（有格式规范）

l 在java程序中调用框架API

# **[了解]软件分层及常用框架**

## **什么是分层**

分层就是把不同功能代码放到不同文件当中。本质是代码的拆分

l Model1模式：jsp+javabean（jsp处理请求）

l Model2模式（MVC）：jsp+serlvet+javabean（servlet处理请求）

n M：模型（service+dao）

n V: 视图

n C: controller

l 经典三层

web表现层（视图+controller）、service层、dao层

## **分层的好处**

代码清晰、解耦、便于维护，出现问题容易定位

## **分层开发下的常见框架**

dao层：hibernate、mybatis

service层：spring框架（对象容器）

web表现层：springmvc框架

# **[了解]原生JDBC操作数据库分析**

l 创建数据库

utf8_general_ci：ci大小写不敏感

select * from user where username='admin' 可以同时查出大写ADMIN的那条记录

l 初始化user表测试数据

l 需求：查询user表所有数据，以List集合形式返回

l 编写POJO类User.java

domain、entity、pojo，本质都一样（一堆的属性和getter/setter）

l 原生JDBC操作数据库存在的问题

```
package com.mybatis.dao;

import com.mybatis.pojo.User;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

public class UserDaoImpl implements UserDao {

   private String driver = "com.mysql.jdbc.Driver";
   private String url = "jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8";
   private String username = "root";
   private String password = "root";


   public List<User> queryUserList() throws Exception {
      List<User> userList = new ArrayList<User>();

      Class.forName(driver);
      /**
       * 问题一：频繁获取/释放数据库连接，影响数据库和应用性能
       * 解决：数据库连接池技术，C3P0,DRUID（阿里巴巴荣誉出品，号称前无古人后无来者世界最强没有之一）
       */
      Connection connection = DriverManager.getConnection(url, username, password);
      /**
       * 问题二：sql语句硬编码，后期难以维护
       * 解决：若sql语句和java代码分离，比如sql写在配置文件中。Mybatis就是这么干的
       */
      String sql = "select * from user";
      /**
       * 问题三：sql语句where条件和占位符一一对应，后期难以维护
       */
      // String sql1 = "select * from user where username=? and id=?";
      PreparedStatement preparedStatement = connection.prepareStatement(sql);
      // preparedStatement.setInt(1,2);
      ResultSet resultSet = preparedStatement.executeQuery();
      User user = null;
      /**
       * 问题四：结果集解析麻烦，查询列硬编码
       * 期望：如果单条记录直接返回实体对象，如果多条记录返回实体的集合
       */
      while(resultSet.next()) {
         user = new User();
         user.setId(resultSet.getInt("id"));
         user.setUsername(resultSet.getString("username"));
         user.setSex(resultSet.getString("sex"));
         user.setBirthday(resultSet.getDate("birthday"));
         user.setAddress(resultSet.getString("address"));

         userList.add(user);
      }
      resultSet.close();
      preparedStatement.close();
      connection.close();
      return userList;
   }
}
```

 

# **[了解]Mybatis框架概述**

​	Mybatis原本是Apache软件基金会的一个开源项目叫做iBatis，2010年这个项目由Apache迁移到了goole code管理才改名为Mybatis，2013年又迁移到了GitHub。

​	注意：招聘信息和面试时候可能问你会ibatis吗

​	Mybatis是一个优秀的持久层框架（Dao层框架），它是对JDBC的封装，使得开发者只需要关注Sql语句（业务）本身即可，无需开发者处理加载驱动、获取连接、创建Statement等繁琐的过程。

​	Mybatis最大的特点是把Sql语句写在XML配置文件当中。而且Mybatis执行完Sql语句之后可以以对象形式返回（POJO/POJO集合等）

​	Mybatis是一个实现了ORM思想的持久层框架

ORM：Object/Relation Mapping 对象/关系映射

ORM思想：将数据库中的关系数据表映射为JAVA中的对象，把对数据表的操作转换为对对象的操作，实现面向对象编程。因此ORM的目的是使得开发人员以面向对象的思想来操作数据库

比如：原来insert使用的是insert into…，如果使用实现了ORM思想的持久层框架，就可以在Java程序中直接调用api，比如insert(User)，达到操作对象即操作数据库的效果

​	Hibernate框架是一个全自动的ORM持久层框架，只需要编写POJO，在xml中定义好Pojo属性和数据表字段的映射/对应关系，就可以在java中实现类似 insert(User)的操作。Sql语句都不用写。但是因为性能等问题，市场占有率越来越低

Mybatis框架是一个半自动的ORM持久层框架，也可以在Java中实现类似 insert(User)的操作最终操作数据库，但是需要我们自己写Sql语句。Mybatis是目前比较流行的Dao层框架。

# **[理解]自定义Mybatis框架**

自定义Mybatis框架开发前的共识：

l 所有的Dao层框架都是以接口的形式给我们提供增删改查的API

l 我们今天自定义Mybatis框架只完成一个API接口：selectList

l 源码开发 —> 打jar包并安装到本地maven仓库 —> 其他项目引用自定义框架

## **开发自定义Mybatis框架**

(1) 步骤1：创建maven工程，packing为jar

(2) 步骤2：定义框架对外API接口类，接口类中只定义一个selectList方法

```
package frame.core;

import java.util.List;

/**
 * 框架对外接口类
 */
public interface SqlSession {

   <T> List<T> selectList() throws Exception;
}

package frame.core;

      import java.util.List;

/**
 * 框架对外接口的实现类
 */
public class SqlSessionImpl implements SqlSession {
   @Override
   public <T> List<T> selectList() throws Exception {
      return null;
   }
} 
```

(3) 步骤:3：最后真正使用框架的时候需要new这个接口的实现类SqlSessionImpl，但是直接new接口的实现类不便于后期维护（如果实现类名字发生变化或者变更实现类，那么系统中众多调用的地方都需要改），因此使用工厂设计模式去帮我们返回接口的实现类对象，这样如果实现类发生变化，我们只需要修改工厂类中一个地方代码即可，便于维护。

```
package frame.factory;

import frame.core.SqlSession;
import frame.core.SqlSessionImpl;

/**
 * 工厂类，用于返回SqlSession的实现类对象
 */
public class SqlSessionFactory {

    public SqlSession openSession() {
        SqlSessionImpl sqlSession = new SqlSessionImpl();
        return sqlSession;
    }
}
```

（4）步骤4：Dao层框架终究是要操作数据库的，底层调用的就是Jdbc，那么Jdbc要做的事情（注册驱动、获取连接等）在框架里一件也都不少，这些事情，我们需要在SqlSessionImpl这个接口实现类中完成（注意：主线还是按照JDBC来的，只是做了一般化处理，为了适应更多的业务场景，不仅是User表）

```
package frame.core;

import java.lang.reflect.Method;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

/**
 * 框架对外接口的实现类
 */
public class SqlSessionImpl implements frame.core.SqlSession {

    // TODO 待办事项
    private String driver;
    private String url;
    private String username;
    private String password;


    @Override
    public <T> List<T> selectList() throws Exception {
        List<T> list = new ArrayList<>();

        Class.forName(driver);
        Connection connection = DriverManager.getConnection(url,username,password);

        // TODO 待办事项
        String sql = "";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        ResultSet resultSet = preparedStatement.executeQuery();

        ResultSetMetaData metaData = resultSet.getMetaData();
        int columnCount = metaData.getColumnCount();

        List<String> columnLabels = new ArrayList<>();
        for(int i = 1; i <= columnCount; i++) {
            columnLabels.add(metaData.getColumnLabel(i));
        }

        // 需要知道结果集实体Pojo的全限定类名，比如:com.mybatis.pojo.User，然后使用反射技术处理
        // TODO 待办事项
        String resultType = "";
        Class aClass = Class.forName(resultType);

        Method[] methods = aClass.getMethods();
        // 取代User user = null;
        Object obj = null;
        while(resultSet.next()) {
            // 取代user = new User();
            obj = aClass.newInstance();
            // 取代user.setUsername(resultSet.getString("username"));
            for(String columnLabel : columnLabels) {
                for(Method method : methods) {
                    if(method.getName().equalsIgnoreCase("set" + columnLabel)) {
                        method.invoke(obj,resultSet.getObject(columnLabel));
                    }
                }

                list.add((T) obj);
            }
        }

        resultSet.close();
        preparedStatement.close();
        connection.close();

        return list;
    }
}
```

（5）步骤5：解决第一个TODO待办事项（数据库信息的获取）

l 定义xml，约定其格式（规范）

![1576427111151](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576427111151.png)

l 定义xml对应的pojo类Configuration，并修改SqlSessionImpl中的部分代码

 ![1576427162874](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576427162874.png)

l 读取xml然后封装pojo对象供项目使用，考虑放在SqlSessionImpl和SqlSessionFactory中都不合适因为会被读取多次，所以我们再抽象一层SqlSessionFactoryBuilder类去构建工厂，构建工厂的同时，准备工厂内部所需要的材料：读取一次xml文件，然后一直使用

| ![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/wps115.jpg) |
| ------------------------------------------------------------ |
|                                                              |

```
package frame.factory;

import frame.pojo.Configuration;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.InputStream;
import java.util.List;

/**
 * 构建工厂，并在这里读取一次xml，避免xml重复读取
 */
public class SqlSessionFactoryBuilder {

    /**
     * 构建工厂的入口，让用户传入配置文件的文件流
     * @param inputStream
     * @return
     */
    public SqlSessionFactory build(InputStream inputStream){
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactory();
        // 读取数据源信息xml为Configuration对象
        Configuration configuration = loadXmlConfig(inputStream);
        sqlSessionFactory.setConfiguration(configuration);
        return sqlSessionFactory;
    }

    /**
     * 读取xml信息为Configuration对象
     * @param inputStream
     * @return
     */
    private Configuration loadXmlConfig(InputStream inputStream){
        Configuration configuration = new Configuration();
        SAXReader saxReader = new SAXReader();
        try {
            Document document = saxReader.read(inputStream);
            Element rootElement = document.getRootElement();
            List<Element> selectNodes = rootElement.selectNodes("//property");

            if(selectNodes != null && selectNodes.size() > 0) {
                for (int i = 0; i < selectNodes.size(); i++) {
                    Element element =  selectNodes.get(i);
                    String name = element.attributeValue("name");
                    String value = element.attributeValue("value");

                    if("driver".equalsIgnoreCase(name)) {
                        configuration.setDriver(value);
                    }
                    if("url".equalsIgnoreCase(name)) {
                        configuration.setUrl(value);
                    }
                    if("username".equalsIgnoreCase(name)) {
                        configuration.setUsername(value);
                    }
                    if("password".equalsIgnoreCase(name)) {
                        configuration.setPassword(value);
                    }
                }
            }
        } catch (DocumentException e) {
            e.printStackTrace();
        }

        return configuration;
    }
}
```

（6）步骤6：处理第二个和第三个待办事项（sql和resultType的获取）

l 定义xml，约定其格式（规范）

```
<?xml version="1.0" encoding="utf-8" ?>
<mapper namespace="test">
<select id="queryUserList" resultType="com.mybatis.pojo.User">
        SELECT * FROM USER;
</select>
</mapper>
```

namespace：分类管理sql的作用，类似于java中的包名

id：标识sql语句

resultType：封装结果的全限定类名

l 定义对应的pojo类Mapper，并且修改Configuration类

```
package frame.pojo;

public class Mapper {

    private String sql;
    private String resultType;

    public String getSql() {
        return sql;
    }

    public void setSql(String sql) {
        this.sql = sql;
    }

    public String getResultType() {
        return resultType;
    }

    public void setResultType(String resultType) {
        this.resultType = resultType;
    }

    @Override
    public String toString() {
        return "Mapper{" +
                "sql='" + sql + '\'' +
                ", resultType='" + resultType + '\'' +
                '}';
    }
} 
```

l 读取配置sql语句的xml文件，在数据源xml中配置<mappers>关联sql配置文件的路径，这样的话通过一个文件流将所有xml信息读入到应用当中（我们不可能提供很多个文件流入口）

![1576427400985](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576427400985.png)

l 修改SqlSessionFactoryBuilder类中读取xml文件的部分

```
package frame.factory;

import frame.pojo.Configuration;
import frame.pojo.Mapper;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.InputStream;
import java.util.List;

/**
 * 构建工厂，并在这里读取一次xml，避免xml重复读取
 */
public class SqlSessionFactoryBuilder {

    /**
     * 构建工厂的入口，让用户传入配置文件的文件流
     * @param inputStream
     * @return
     */
    public SqlSessionFactory build(InputStream inputStream){
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactory();
        // 读取数据源信息xml为Configuration对象
        Configuration configuration = loadXmlConfig(inputStream);
        sqlSessionFactory.setConfiguration(configuration);
        return sqlSessionFactory;
    }

    /**
     * 读取xml信息为Configuration对象
     * @param inputStream
     * @return
     */
    private Configuration loadXmlConfig(InputStream inputStream){
        Configuration configuration = new Configuration();
        SAXReader saxReader = new SAXReader();
        try {
            Document document = saxReader.read(inputStream);
            Element rootElement = document.getRootElement();
            List<Element> selectNodes = rootElement.selectNodes("//property");

            if(selectNodes != null && selectNodes.size() > 0) {
                for (int i = 0; i < selectNodes.size(); i++) {
                    Element element =  selectNodes.get(i);
                    String name = element.attributeValue("name");
                    String value = element.attributeValue("value");

                    if("driver".equalsIgnoreCase(name)) {
                        configuration.setDriver(value);
                    }
                    if("url".equalsIgnoreCase(name)) {
                        configuration.setUrl(value);
                    }
                    if("username".equalsIgnoreCase(name)) {
                        configuration.setUsername(value);
                    }
                    if("password".equalsIgnoreCase(name)) {
                        configuration.setPassword(value);
                    }
                }
            }


            List<Element> mappers = rootElement.selectNodes("//mapper");

            if(mappers != null && mappers.size() > 0) {
                for (int i = 0; i < mappers.size(); i++) {
                    Element element =  mappers.get(i);
                    String mapperPath = element.attributeValue("resource");
                    loadSqlXmlConfig(configuration,mapperPath);
                }
            }
        } catch (DocumentException e) {
            e.printStackTrace();
        }

        return configuration;
    }

    /**
     * 读取存放sql的xml文件
     * @param mapperPath
     */
    private void loadSqlXmlConfig(Configuration configuration,String mapperPath) {
        InputStream resourceAsStream = this.getClass().getClassLoader().getResourceAsStream(mapperPath);
        SAXReader saxReader = new SAXReader();
        try {
            Document document = saxReader.read(resourceAsStream);
            Element rootElement = document.getRootElement();
            String namespace = rootElement.attributeValue("namespace");
            List<Element> selectNodes = rootElement.selectNodes("//select");
            if(selectNodes != null && selectNodes.size() > 0) {
                for (int i = 0; i < selectNodes.size(); i++) {
                    Element element =  selectNodes.get(i);
                    String id = element.attributeValue("id");
                    String resultType = element.attributeValue("resultType");
                    String sql = element.getText();
                    Mapper mapper = new Mapper();
                    mapper.setSql(sql);
                    mapper.setResultType(resultType);
                    configuration.getMapperMap().put(namespace + "." + id,mapper);
                }
            }
        } catch (DocumentException e) {
            e.printStackTrace();
        }
    }
}
```

 

（7）步骤7：修改SqlSessionImpl中第二和第三个待办事项

```
package frame.core;

import javax.security.auth.login.Configuration;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

/**
 * 框架对外接口类
 */
public interface SqlSession {

    <T> List<T> selectList(String sqlId) throws Exception;
}

package frame.core;

        import frame.pojo.Configuration;

        import java.lang.reflect.Method;
        import java.sql.*;
        import java.util.ArrayList;
        import java.util.List;

/**
 * 框架对外接口的实现类
 */
public class SqlSessionImpl implements SqlSession {

    private Configuration configuration;
    public void setConfiguration(Configuration configuration) {
        this.configuration = configuration;
    }

    @Override
    public <T> List<T> selectList(String sqlId) throws Exception {
        List<T> list = new ArrayList<>();

        Class.forName(configuration.getDriver());
        Connection connection = DriverManager.getConnection(configuration.getUrl(),
                configuration.getUsername(),configuration.getPassword());

        String sql = configuration.getMapperMap().get(sqlId).getSql();
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        ResultSet resultSet = preparedStatement.executeQuery();

        ResultSetMetaData metaData = resultSet.getMetaData();
        int columnCount = metaData.getColumnCount();

        List<String> columnLabels = new ArrayList<>();
        for(int i = 1; i <= columnCount; i++) {
            columnLabels.add(metaData.getColumnLabel(i));
        }

        // 需要知道结果集实体Pojo的全限定类名，比如:com.mybatis.pojo.User，然后使用反射技术处理
        String resultType = configuration.getMapperMap().get(sqlId).getResultType();
        Class aClass = Class.forName(resultType);

        Method[] methods = aClass.getMethods();
        // 取代User user = null;
        Object obj = null;
        while(resultSet.next()) {
            // 取代user = new User();
            obj = aClass.newInstance();
            // 取代user.setUsername(resultSet.getString("username"));
            for(String columnLabel : columnLabels) {
                for(Method method : methods) {
                    if(method.getName().equalsIgnoreCase("set" + columnLabel)) {
                        method.invoke(obj,resultSet.getObject(columnLabel));
                    }
                }

                list.add((T) obj);
            }
        }

        resultSet.close();
        preparedStatement.close();
        connection.close();

        return list;
    }
} 
```

步骤8：将SqlSessionFactoryBuilder中读取xml构建Configuration对象的代码抽取为XmlConfigBuilder类，便于代码重用（不同功能的代码放到不同的java类中，各司其职）。

构建Configuration复杂对象的过程也是设计模式之构建者模式的一种体现。（构建者模式参见扩展知识部分）

l XmlConfigBuilder.java类

```
package frame.utils;

import frame.pojo.Configuration;
import frame.pojo.Mapper;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.InputStream;
import java.util.List;

public class XmlConfigBuilder {

    /**
     * 读取xml信息为Configuration对象
     * @param inputStream
     * @return
     */
    public Configuration loadXmlConfig(InputStream inputStream){
        Configuration configuration = new Configuration();
        SAXReader saxReader = new SAXReader();
        try {
            Document document = saxReader.read(inputStream);
            Element rootElement = document.getRootElement();
            List<Element> selectNodes = rootElement.selectNodes("//property");

            if(selectNodes != null && selectNodes.size() > 0) {
                for (int i = 0; i < selectNodes.size(); i++) {
                    Element element =  selectNodes.get(i);
                    String name = element.attributeValue("name");
                    String value = element.attributeValue("value");

                    if("driver".equalsIgnoreCase(name)) {
                        configuration.setDriver(value);
                    }
                    if("url".equalsIgnoreCase(name)) {
                        configuration.setUrl(value);
                    }
                    if("username".equalsIgnoreCase(name)) {
                        configuration.setUsername(value);
                    }
                    if("password".equalsIgnoreCase(name)) {
                        configuration.setPassword(value);
                    }
                }
            }
            List<Element> mappers = rootElement.selectNodes("//mapper");

            if(mappers != null && mappers.size() > 0) {
                for (int i = 0; i < mappers.size(); i++) {
                    Element element =  mappers.get(i);
                    String mapperPath = element.attributeValue("resource");
                    loadSqlXmlConfig(configuration,mapperPath);
                }
            }
        } catch (DocumentException e) {
            e.printStackTrace();
        }

        return configuration;
    }

    /**
     * 读取存放sql的xml文件
     * @param mapperPath
     */
    private void loadSqlXmlConfig(Configuration configuration,String mapperPath) {
        InputStream resourceAsStream = this.getClass().getClassLoader().getResourceAsStream(mapperPath);
        SAXReader saxReader = new SAXReader();
        try {
            Document document = saxReader.read(resourceAsStream);
            Element rootElement = document.getRootElement();
            String namespace = rootElement.attributeValue("namespace");
            List<Element> selectNodes = rootElement.selectNodes("//select");
            if(selectNodes != null && selectNodes.size() > 0) {
                for (int i = 0; i < selectNodes.size(); i++) {
                    Element element =  selectNodes.get(i);
                    String id = element.attributeValue("id");
                    String resultType = element.attributeValue("resultType");
                    String sql = element.getText();
                    Mapper mapper = new Mapper();
                    mapper.setSql(sql);
                    mapper.setResultType(resultType);
                    configuration.getMapperMap().put(namespace + "." + id,mapper);
                }
            }
        } catch (DocumentException e) {
            e.printStackTrace();
        }
    }
}
```

修改SqlSessionFactoryBuilder类

```
package frame.factory;

import frame.pojo.Configuration;
import frame.utils.XmlConfigBuilder;

import java.io.InputStream;

/**
 * 构建工厂，并在这里读取一次xml，避免xml重复读取
 */
public class SqlSessionFactoryBuilder {

   /**
    * 构建工厂的入口，让用户传入配置文件的文件流
    * @param inputStream
    * @return
    */
   public SqlSessionFactory build(InputStream inputStream){
      SqlSessionFactory sqlSessionFactory = new SqlSessionFactory();
      // 读取数据源信息xml为Configuration对象
      XmlConfigBuilder xmlConfigBuilder = new XmlConfigBuilder();
      Configuration configuration = xmlConfigBuilder.loadXmlConfig(inputStream);
      sqlSessionFactory.setConfiguration(configuration);
      return sqlSessionFactory;
   }
}
```



## **打包测试**

### **打包**

l 双击install，安装到本地仓库

 ![1576427752911](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576427752911.png)

l 本地Maven仓库查看是否打包成功

### **测试**

注意：新建一个工程进行测试，否则在同一个工程中的话引用的是自定义Mybatis框架的源码工程，而非打好的jar包。

## **自定义Mybatis框架小结**

l 自定义Mybatis结构图

 ![1576427774271](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576427774271.png)

l 框架的开发使用流程

编写源代码—>打jar包—>框架使用者引入jar包—>按照框架的规范进行XML配置—>Java程序中调用框架API实现功能

l 引入框架坐标即可，它所依赖的dom4j和jaxen都不需要我们引入，这是Maven的依赖传递

l 框架是基础知识的整合（本课程中的jdbc、dom4j、xpath、反射、数据库元数据等）

l 框架往往都会用到设计模式（本课程扩展了工厂设计模式和构建者设计模式）

l 框架展示了代码的拆分思想，各司其职，便于维护

# **[掌握]Mybatis框架快速入门**

l 下载Mybatis

 ![1576427785633](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576427785633.png)

![1576427797767](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576427797767.png)

我们本次课程使用3.4.5版本

l Mybatis入门程序

查询user表全部记录，以list集合形式返回

l 测试程序

n 引入jar

```
<dependencies>
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>4.12</version>
</dependency>

<dependency>
<groupId>org.mybatis</groupId>
<artifactId>mybatis</artifactId>
<version>3.4.5</version>
</dependency>

<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>5.1.46</version>
</dependency>

<dependency>
<groupId>log4j</groupId>
<artifactId>log4j</artifactId>
<version>1.2.17</version>
</dependency>
</dependencies>
```

 

n pojo类User.java

```
package com.mybatis.pojo;

import java.util.Date;

public class User {

    private Integer id;
    private String username;
    private String sex;
    private Date birthday;
    private String address;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", sex='" + sex + '\'' +
                ", birthday=" + birthday +
                ", address='" + address + '\'' +
                '}';
    }
}
```

n SqlMapConfig.xml

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
      PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
      "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<environments default="development">
<environment id="development">
<transactionManager type="JDBC" />
<dataSource type="POOLED">
<property name="driver" value="com.mysql.jdbc.Driver" />
<property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?characterEncoding=utf8" />
<property name="username" value="root" />
<property name="password" value="root" />
</dataSource>
</environment>
</environments>

<mappers>
<mapper resource="UserMapper.xml"></mapper>
</mappers>
</configuration>    
```

n UserMapper.xml

```
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE mapper
      PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
      "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="test">
<select id="queryUserList" resultType="com.mybatis.pojo.User">
      select * from user
</select>
</mapper>
```

n 测试程序

```
@Test
public void testMybatis() throws IOException {
      InputStream inputStream = Resources.getResourceAsStream("SqlMapConfig.xml");
      SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
      SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
      SqlSession sqlSession = sqlSessionFactory.openSession();
      List<User> list = sqlSession.selectList("test.queryUserList");
      if(list != null && list.size() > 0) {
      for (int i = 0; i < list.size(); i++) {
      User user =  list.get(i);
      System.out.println(user);
      }
      }
      sqlSession.close();
      }
```

# **[了解]扩展知识**

## **设计模式**

设计模式是前人代码经验的总结，是别人优秀的代码写法（写代码的套路），适当的场景使用设计模式提高代码的健壮性、可扩展和可维护性

l 工厂设计模式

n 工厂模式是一种比较常用的设计模式，是帮我们实例化对象的（即new对象的），所以以后new对象的时候你要考虑是否可以使用工厂模式

n 工厂模式下常用的有简单工厂模式和工厂方法模式两种（参考我提供给大家的demo工程）

l 构建者（build）模式

构建者模式，又称建造者模式，是将一个复杂对象的构建分为许多小对象的构建，最后在整合在一起的模式。

n Builder类：定义组装的对象包括哪些部分（硅谷大表哥）

n Builder实现类：具体组装各个小部分的类（具体干活员工）

n Director类：指导Builder实现类组装的类（员工的师傅，指导干活的人）

l 示例代码参考

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/wps122.jpg) 

## **元数据**

数据表是用来存储我们业务数据的，而元数据是用来描述数据表的，比如这个表的表结构，有哪些字段等信息。本节课我们要知道查询结果集中有哪些数据项就可以通过元数据技术获取。

**MetaData：元数据的意思**

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/wps123.jpg) 

## **log4j日志组件**

Log4j就是一种日志组件，可以把日志打印到控制台，也可以输出到文件

log4j使用步骤（和框架使用类似）：

l 导入log4j的jar包

​	<dependency>

​        <groupId>log4j</groupId>

​        <artifactId>log4j</artifactId>

​        <version>1.2.17</version>

</dependency>

l 在classpath下创建log4j.properties配置文件并进行配置（文件名称是固定为log4j.properties）

l Java代码中调用API输出日志，例如

![1576427979777](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576427979777.png)

l 示例代码参考

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/wps125.jpg) 

# **注意：所有Maven工程指定为JDK1.8编译**

```
<build>
<plugins>
<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-compiler-plugin</artifactId>
<version>3.1</version>
<configuration>
<source>1.8</source>
<target>1.8</target>
<encoding>UTF-8</encoding>
</configuration>
</plugin>
</plugins>
</build>
```

# **[理解]关于三个对象**

l SqlSessionFactoryBuilder： 创建工厂，生命周期短暂即可，类似于一个工具类

l SqlSessionFactory： 应该只有一个工厂对象即可。

l SqlSession： 和数据库通信的会话对象，底层对应connection连接。例如：假如多个线程公用一个sqlsession对象（connection），线程A操作了数据库尚未提交事务，线程B操作了数据库把这个connecttion的事务给提交了，会造成数据安全问题。所以一个线程应该有自己的一个sqlsession对象。

换言之，SqlSession是线程不安全的

# **[掌握]Mybatis入门级CRUD操作** 

功能需求：

基于已有数据表user，使用MyBatis实现以下功能：

n 根据用户id查询一个用户

n 根据用户名称模糊查询用户列表

n 添加一个用户

n 根据用户id修改用户名

n 根据用户id删除用户

```
<dependencies>
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>4.12</version>
</dependency>

<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>5.1.46</version>
</dependency>
<!--mybatis的jar坐标-->
<dependency>
<groupId>org.mybatis</groupId>
<artifactId>mybatis</artifactId>
<version>3.4.5</version>
</dependency>
<!--log4j日志组件，要同时把兄弟log4j.properties引入-->
<dependency>
<groupId>log4j</groupId>
<artifactId>log4j</artifactId>
<version>1.2.17</version>
</dependency>
</dependencies>

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

组件：

POJO

SqlMapConfig.xml

新增业务，修改：UserMapper.xml

关联SqlMapConfig和UserMapper

新增业务，修改：Java程序调用（测试程序）

## **根据用户id查询用户**

l UserMapper.xml

```
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE mapper
      PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
      "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--mapper根标签，映射的意思
      namespace：命名空间，分类管理的sql的作用
      -->
<mapper namespace="test">

<!--
      查询使用select标签
      parameterType：传入的参数类型
      resultType：封装的结果类型
      -->
<select id="queryUserById" parameterType="Integer" resultType="com.mybatis.pojo.User">
<!--
      #{}固定取参语法
      #{}取参名称：当parameterType传入简单数据类型的时候，取参名称任意，但是建议见名思意，比如是id就#{id}
      -->
      select * from user where id=#{id}
</select>

</mapper>
```

l 测试程序

```
/**
 * 测试用例(Use_case)：根据id查询用户
 */
@Test
public void testQueryUserById() throws IOException {
      InputStream inputStream = Resources.getResourceAsStream("SqlMapConfig.xml");
      SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
      SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
      SqlSession sqlSession = sqlSessionFactory.openSession();
      /*
       * 调用会话对象的api完成crud
       * param1：锁定sql的唯一标识符
       * param2：传入的参数
       * */
      User user = sqlSession.selectOne("test.queryUserById",1);
      System.out.println(user);
      sqlSession.close();
      inputStream.close();
      }
```

 

## **根据用户名模糊查询用户列表**

l #{}方式取参

n UserMapper.xml

```
<!--根据用户名模糊查询用户列表-->
<!--注意：返回多条记录和返回单条记录，resultType配置是一样的-->
<select id="queryUserByUsername" parameterType="String" resultType="com.mybatis.pojo.User">
<!--
      #{}底层原理：预编译语句（防止sql注入，性能提高）
      #{}对字符串参数自动添加单引号对
      -->
      select * from user where username like #{username}
</select>
```

n 测试程序

```
/**
 * 测试用例(Use_case)：根据用户名模糊查询用户列表
 */
@Test
public void testQueryUserByUsername() throws IOException {
      InputStream inputStream = Resources.getResourceAsStream("SqlMapConfig.xml");
      SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
      SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
      SqlSession sqlSession = sqlSessionFactory.openSession();
      // TooManyResultsException: Expected one result (or null) to be returned by selectOne(), but found: 2
      List<User>  userList = sqlSession.selectList("test.queryUserByUsername","%王%");
      if(userList != null && userList.size() > 0) {
      for (int i = 0; i < userList.size(); i++) {
      User user =  userList.get(i);
      System.out.println(user);
      }
      }
      sqlSession.close();
      inputStream.close();
      }
```

l ${}方式取参

n UserMapper.xml

```
<!--
      ${}：另外一种取参方式
      ${}取参名称：当parameterType传入简单数据类型的时候，取参名称必须为value字符串
      ${}原理：简单的字符串，传什么参数拼接什么
      不会对字符串参数添加单引号对
      -->

<!--
      #和$取参使用原则
      能用#不用$(从底层原理角度出发说的，#是预编译语句),特殊情况使用$：当你传入的参数是数据库对象（比如表名动态传入），或者是
      order by 的那个字段，都不希望自动添加单引号对，所以使用$
      -->
<select id="queryUserByUsername$" parameterType="String" resultType="com.mybatis.pojo.User">
      select * from user where username like ${value}
</select>
```

n 测试程序

 * ```
    /**
    
     - 测试用例(Use_case)：根据用户名模糊查询用户列表$取参形式
     */
    @Test
    public void testQueryUserByUsername$() throws IOException {
            InputStream inputStream = Resources.getResourceAsStream("SqlMapConfig.xml");
            SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
            SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
            SqlSession sqlSession = sqlSessionFactory.openSession();
            // TooManyResultsException: Expected one result (or null) to be returned by selectOne(), but found: 2
            List<User>  userList = sqlSession.selectList("test.queryUserByUsername$","'%王%'");
            if(userList != null && userList.size() > 0) {
            for (int i = 0; i < userList.size(); i++) {
            User user =  userList.get(i);
            System.out.println(user);
            }
            }
            sqlSession.close();
            inputStream.close();
            }
    ```

## **小结**

查询部分小结：

l selectOne，当结果集有多条记录的时候，抛出异常；

l #{}和${}使用原则

能使用#不用$，特殊情况下使用$，特殊情况比如传入的参数是数据库对象（表名）或者order by的字段名时，不希望自动添加单引号对；

l 返回结果集列表的时候，resultType和返回单个对象配置都一样；

## **实现添加用户（及Mysql自增主键返回）**

```
<!--
        #{}和${}取参时，当parameterType为pojo的时候，取参名称为pojo的属性名
        -->
<insert id="saveUser" parameterType="com.mybatis.pojo.User">

<!--
        selectKey用于查询主键数据
        order：selectKey中的sql在insert之前还是之后执行 AFTER/BEFORE
        resultType:查询出的主键的类型
        keyProperty：查询出的主键数据回显到pojo的哪个属性
        -->
<selectKey order="AFTER" resultType="Integer" keyProperty="id">
        select last_insert_id()
</selectKey>
        insert into user(username,sex,birthday,address)
        values(#{username},#{sex},#{birthday},#{address})
</insert>
```

自增主键返回：其实就是mybaits在执行完insert之后立马查询last_insert_id()

## **根据用户id修改用户名**

 <update id="updateUsernameById" parameterType="com.mybatis.pojo.User">        update user set username=#{username} where id=#{id}</update>

## **根据用户id删除用户**

 <delete id="deleteUserById" parameterType="Integer">        delete from user where id=#{id}</delete>

## **Mybatis解决原生JDBC操作数据库存在的问题**

1、频繁创建、释放数据库连接造成系统资源浪费，影响系统性能。使用数据库连接池技术可以解决此问题。

解决：在SqlMapConfig.xml中配置数据连接池，使用连接池管理数据库连接。

2、Sql语句写在代码中造成代码不易维护，实际应用中Sql变化的可能较大，Sql变动需要改变java代码。

解决：将Sql语句配置在XXXXmapper.xml文件中与Java代码分离。

3、向Sql语句传参数麻烦，因为Sql语句的where条件不一定，可能多也可能少，占位符需要和参数一一对应（硬编码）。

解决：Mybatis自动将Java对象映射至Sql语句，通过statement中的parameterType定义输入参数的类型。

4、对结果集解析麻烦（查询列硬编码），Sql变化会导致解析代码变化，且解析前需要遍历，如果能将数据库记录封装成Pojo对象解析比较方便。

解决：Mybatis自动将Sql执行结果映射至Java对象，通过statement中的resultType定义输出结果的类型。

# **[掌握]Mybatis的两种Dao开发方式**

使用MyBatis开发DAO的方式实现以下的功能：

n 根据用户id查询一个用户信息

## **原始Dao开发方式**

​	定义接口（UserDao）

​	写实现类（UserDaoImpl）：在实现类中写crud那套程序

l 定义接口UserDao

```
package com.mybatis.dao;

import com.mybatis.pojo.User;

/**
 * 原始Dao开发方式接口定义
 */
public interface UserDao {

    User queryUserById(Integer id) throws Exception;
}
```

l 写实现类

```
package com.mybatis.dao;

import com.mybatis.pojo.User;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;

public class UserDaoImpl implements UserDao {

    /**
     * 声明一个SqlSessionFactory变量，用于接收service层调用dao层时传入的工厂对象
     * 因为工厂对象全局唯一，对于全局唯一的对象，在分层的架构中，dao层使用时候应该由
     * 外部传入
     */
    private SqlSessionFactory sqlSessionFactory;
    public UserDaoImpl(SqlSessionFactory sqlSessionFactory) {
        this.sqlSessionFactory = sqlSessionFactory;
    }

    @Override
    public User queryUserById(Integer id) throws Exception {
        SqlSession sqlSession = sqlSessionFactory.openSession();
        User user = sqlSession.selectOne("test.queryUserById", id);
        sqlSession.close();
        return user;
    }
}
```

**注意：分层其实是对原有代码的重新组织**

 

l 思考

一个项目当中会有很多表，都对应dao接口和实现类，实现类就是做增删改查这些东西

能否不写实现类只定义接口，让框架帮我们完成实现类的逻辑，那么就是Mapper动态代理的开发方式

## **Mapper动态代理开发方式**

l 定义一个Mapper接口，这个接口其实和我们UserDao接口是一样的

 

Mapper接口中的接口方法被调用时候，最终要找到这个接口方法所使用的sql语句？那如何找到呢？

遵从下面4个规范即可

l 从Mybatis框架中拿到一个代理对象（代理的是这个Mapper接口），通过代理对象调用接口当中的方法完成业务

n 传统dao开发方式中的实现类其实起了一个连接、承上启下的作用，连接了接口和xml映射文件，效果就是调用接口方法时能够找到xml映射文件

n Mapper动态代理开发遵从的规范

u sql映射文件的namespace必须和mapper接口的全限定类名保持一致

u mapper接口的接口方法名必须和xml中的sql语句id保持一致

u mapper接口的接口方法形参类型必须和sql语句的输入参数类型保持一致

u mapper接口的接口方法返回类型必须和sql语句的resultType保持一致

## **小结**

原始Dao和Mapper动态代理开发在企业中都很常见，推荐使用Mapper动态代理开发模式

# **[掌握]全局配置文件SqlMapConfig.xml的使用说明**

注意：子标签是有顺序要求的，因为使用了dtd校验

## **properties（属性）**

引入外部属性文件

 <!--        properties：引入外部资源文件        resource:指向classpath下的资源文件路径        注意事项：优先级问题，外部引入的资源文件的配置项的优先级   高于  子标签property配置信息        --><properties resource="db.properties"><property name="jdbc.username" value="root1"/></properties>

## **typeAliases（类型别名）**

### **Mybatis默认支持的别名**

| 别名       | 映射的类型 |
| ---------- | ---------- |
| _byte      | byte       |
| _long      | long       |
| _short     | short      |
| _int       | int        |
| _integer   | int        |
| _double    | double     |
| _float     | float      |
| _boolean   | boolean    |
| string     | String     |
| byte       | Byte       |
| long       | Long       |
| short      | Short      |
| int        | Integer    |
| integer    | Integer    |
| double     | Double     |
| float      | Float      |
| boolean    | Boolean    |
| date       | Date       |
| decimal    | BigDecimal |
| bigdecimal | BigDecimal |
| map        | Map        |

注意：更多默认别名参见mybatis源码中的**TypeAliasRegistry**类

### **自定义别名**

 <!--      typeAliases定义别名，主要给pojo定义别名      --><typeAliases><!--      type：给哪个类定义别名      alias：别名名称      --><!--<typeAlias type="com.mybatis.pojo.User" alias="user"/>--><!--批量定义别名：package指定要扫描的包路径      注意：package会扫描包及其子包下的所有类，不要类名相重复的情况，否则异常      --><package name="com.mybatis.pojo"/></typeAliases>

## **mappers（映射器）**

引入sql语句的xml配置文件，把sql语句加载到内存当中

l <mapper resource=""/>  万能型选手，基础crud、原始dao开发、mapper动态代理开发都可以使用这种方式

l 针对mapper动态代理开发dao层，我们进行一个增加

mapper class属性：执行mapper接口类的全限定类名

​	前提要求：

​	1、编译之后 mapper.xml和mapper.class在同一个目录文件夹中

2、两个文件名称相同

 <!--      规范：编译之后，UserMapper接口类的class要和UserMapper.xml在同一个目录中，而且文件名相同      --><mapper class="com.mybatis.mapper.UserMapper"/><!--批量扫描mapper接口类，其实是扫描到包下面的一个个类，然后再按照<mapper class的形式去加载      所以，package扫描的要求（规范）和直接使用class属性是一样的      --><package name="com.mybatis.mapper"/>

# **[掌握]Mybatis的输入类型和结果类型**

## **parameterType（输入类型）**

l 传递简单类型

参考上面

l 传递Pojo对象

参考上面

l 传递Pojo包装对象

n 需求：使用POJO包装对象，根据用户名模糊查询用户列表

n 什么是Pojo包装对象

![1576430528201](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576430528201.png)

n 应用在什么场景

![1576430538743](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576430538743.png)

地址：http://epub.sipo.gov.cn/gjcx.jsp

## **resultType（输出类型）**

l 输出简单类型

查询user表总记录数

l 输出Pojo对象

参考上面

l 输出Pojo列表

参考上面

## **resultMap（手动映射）**

l 订单表POJO类

```
package com.mybatis.pojo;

import java.util.Date;

public class Orders {
    // 订单id
    private int id;
    // 用户id
    private Integer userId;
    // 订单号
    private String number;
    // 订单创建时间
    private Date createtime;
    // 备注
    private String note;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public Integer getUserId() {
        return userId;
    }

    public void setUserId(Integer userId) {
        this.userId = userId;
    }

    public String getNumber() {
        return number;
    }

    public void setNumber(String number) {
        this.number = number;
    }

    public Date getCreatetime() {
        return createtime;
    }

    public void setCreatetime(Date createtime) {
        this.createtime = createtime;
    }

    public String getNote() {
        return note;
    }

    public void setNote(String note) {
        this.note = note;
    }

    @Override
    public String toString() {
        return "Orders{" +
                "id=" + id +
                ", userId=" + userId +
                ", number='" + number + '\'' +
                ", createtime=" + createtime +
                ", note='" + note + '\'' +
                '}';
    }
}
```

n OrdersMapper.xml

```java
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mybatis.mapper.OrdersMapper">

<!--
        resultType：自动映射，要求查询列名和pojo属性名保持一致
        1 最终封装的对象
        2 查询列名和pojo属性名一致
        当不一致的时候，要使用手动映射
        resultMap：配置手动映射
        -->
<select id="queryOrdersAllList" resultMap="ordersResultMap">
        select * from orders
</select>


<resultMap id="ordersResultMap" type="orders">
<!--手动配置映射关系-->
<!--
        主键字段不一致的时候使用<id>标签配置
        普通字段不一致的时候使用<result>标签配置
        property严格区分大小写，要和pojo属性名称一致
        -->
<id column="id" property="id"/>
<result column="user_id" property="userId"/>
<result column="number" property="number"/>
<result column="createtime" property="createtime"/>
<result column="note" property="note"/>
</resultMap>


</mapper>
```

# **工程准备**

新建一个Maven类型的module：实现Mybatis第二天的保存用户功能，便于基于该功能的代码程序说明Mybatis数据库连接池和Mybatis事务控制。

# **[理解]Mybatis连接池和事务控制**

## **Mybatis连接池**

l POOLED使用连接池

n 连接池示意

![1576430643964](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576430643964.png)

 

n Mybatis连接池初始化时机

![1576430656070](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576430656070.png)

在构建工厂的时候创建Mybatis的数据库连接池

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/wps130.jpg) 

n 什么时候从连接池获取连接

getMapper的时候还是真正调用api操作数据库的时候？

从连接池获取连接的时机：真正操作数据库调用api的时候，不是getMapper的时候

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/wps131.jpg) 

l 其他

n UNPOOLED：不使用数据库连接池（一般不使用）

n JNDI:(前提你的Mybatis环境必须是Web应用)（了解）

u 什么是JNDI

JNDI:java naming directory interface(java命名目录接口)，它是一种该服务发布技术，可以通过JNDI技术把数据源发布成一个服务，那么客户端可以调用这个服务

u 为什么必须是web应用

因为支持JNDI技术的往往是tomcat/weblogic/websphere这些中间件才支持JNDI技术

u 如果在Mybatis当中用，怎么用

参考附录

## **Mybatis事务控制**

type="JDBC"：使用Jdbc的事务管理机制，connection的事务

Mybatis默认把Jdbc的事务自动提交给关闭了

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/wps132.jpg) 

注意：考虑提交的数据量较小的时候使用自动提交，如果数据量大影响了操作数据库的性能，那么考虑分批次手动控制事务的提交

type="MANAGED"：什么都不做，交给其他框架管理事务



# **[掌握]Mybatis映射文件的Sql深入（即动态Sql）**

更优雅更方便的帮助我们拼接sql语句。

根据用户性别和用户名称查询用户列表

## **if标签/where标签**

 ![1576430718696](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576430718696.png)

## **sql片段**

| ![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/wps134.jpg) |
| ------------------------------------------------------------ |
| ![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/wps135.jpg) |

引用sql片段使用include标签，refid指向sql片段的id，如果共享其他mapper文件当中的sql片段，只需要refid前面加上另外一个mapper映射文件的namespace即可

## **foreach标签**

delete from user where id in(1,2,3)

String sql = "delete from user where id in("

for(String s: list) {

​	sql += s + ","

}

substring

sql += ")"

需求：根据多个id来查询用户列表

select * from user where id in(1,2,3)

l 形式一：直接传入list集合给mybatis

| ![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/wps136.jpg) |
| ------------------------------------------------------------ |
| ![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/wps137.jpg) |

 

l 形式二：直接传入数组给Mybatis

Mybatis底层别名定义类

| ![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/wps138.jpg) |
| ------------------------------------------------------------ |
|                                                              |

```
<!--
        foreach直接传入Array数组
        -->
<select id="queryUserByIdsArray" parameterType="int[]" resultType="user">
        select id,username,sex,birthday,address from user
<!--
        foreach：遍历拼接sql语句
        collection：遍历的集合，注意当直接传数组，除了知道是数组外没有别的名字，所以collection固定配置为array
        open：遍历拼接前干点啥（拼接点什么字符串）
        close:遍历结束后干点啥（拼接点什么字符串）
        separator：遍历拼接时候使用的分隔符
        item：遍历到的具体元素值，等同于下面for循环中的s
        for(String s : list) {
        s
        }
        -->
        <foreach collection="array" open=" where id in(" close=")" separator="," item="item">
        #{item}
        </foreach>
        </select> 
```

l 形式三：传入一个pojo，pojo中有list或者数组

 <!--        foreach直接传入pojo,pojo中封装list或者数组        --><select id="queryUserByIdsQueryVo" parameterType="queryvo" resultType="user">        select id,username,sex,birthday,address from user<!--        foreach：遍历拼接sql语句        collection：遍历的集合，当传入pojo的时候，collection配置为pojo当中集合的属性名        如果不指定具体属性名，那么mybatis是不知道要取pojo哪个属性的        open：遍历拼接前干点啥（拼接点什么字符串）        close:遍历结束后干点啥（拼接点什么字符串）        separator：遍历拼接时候使用的分隔符        item：遍历到的具体元素值，等同于下面for循环中的s        for(String s : list) {        s        }        -->        <foreach collection="idsList" open=" where id in(" close=")" separator="," item="item">        #{item}        </foreach>        </select>

# **[掌握]Mybatis的多表关联查询**

多表：至少2张表

多表关系分析技巧：**从一条记录出发**，比如分析A表和B表的关系，就看A表的一条记录可以对应B表中几条记录，如果对应一条，从A到B表就是一对一的关系，如果对应多条就是一对多的关系。

多对多其实是双向的一对多

l 一对一

从订单表出发到用户表，一条订单记录只会对应用户表的一条记录，一对一

l 一对多

从用户表出发到订单表，一个用户记录可以对应订单表的多条记录，一对多

（通过**主外键**，一的一方是主表，多的一方是从表，外键在从表多的那一方）

l 多对多

用户和角色，一个用户可以有多个角色，一个角色也可以对应多个用户

通过中间表表达两个表之间的关系

a表         b表

id field..    id field..

中间表（往往两个字段即可）

aid  bid

## **多表关联的SQL语句表达分析**

l Sql语句表达

n 笛卡尔积

u SELECT * FROM USER,orders

![1576477877978](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576477877978.png)

SELECT * FROM USER u,orders o WHERE u.id=o.user_id(笛卡尔积进一步筛选)

n 关联查询

u 内关联 innder join on（和笛卡尔积进一步筛选效果一样）

SELECT * FROM USER u INNER JOIN orders o ON u.id=o.user_id

u 外连接(outer join on)（使用较多）

**左外连接(left join)  left join左边的表是基础表**

**SELECT \* FROM USER u LEFT JOIN orders o ON u.id=o.user_id**

右外连接(right join) right join右边的表是基础表

SELECT * FROM USER u RIGHT JOIN orders o ON u.id=o.user_id

l 内关联和外关联分析

![img](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/wps140.jpg) 

## **一对一查询**

Mybatis帮我们做的事情：

1、 SQL语句是用户自己写，Mybatis框架只是帮我们执行而已

2、 封装结果集

 

需求：查询订单表全部数据，关联查询出订单对应的用户数据（username address）

l Mapper映射文件

```
<select id="queryOrdersUser" resultMap="ordersUserResultMap">
        SELECT
        o.`id`,
        o.`user_id`,
        o.`number`,
        o.`createtime`,
        o.`note`,
        u.`username`,
        u.`address`
        FROM
        orders o
        LEFT JOIN USER u
        ON o.`user_id` = u.`id`
</select>


<resultMap id="ordersUserResultMap" type="orders">
<!--两部分数据：订单+用户-->
<id column="id" property="id"/>
<result column="user_id" property="userId"/>
<result column="number" property="number"/>
<result column="createtime" property="createtime"/>
<result column="note" property="note"/>

<!--association：关联的意思,用于一对一关联封装数据
        property：一对一关联后封装成的数据所对应的属性名
        javaType：属性的类型
        -->
<association property="user" javaType="user">
<id column="user_id" property="id"/>
<result column="username" property="username"/>
<result column="address" property="address"/>
</association>
</resultMap> 
```

l 测试程序

```
@Test
public void testQueryOrdersUser() {
        SqlSession sqlSession = sqlSessionFactory.openSession();
        OrdersMapper ordersMapper = sqlSession.getMapper(OrdersMapper.class);
        List<Orders> ordersList = ordersMapper.queryOrdersUser();
        if(ordersList != null && ordersList.size() > 0) {
        for (int i = 0; i < ordersList.size(); i++) {
        Orders orders =  ordersList.get(i);
        System.out.println(orders);
        }
        }
        sqlSession.close();
        }
```

## **一对多查询**

需求：查询全部用户数据，关联查询出订单数据

l Mapper映射文件

```
<!--一对多用例-->
<select id="queryUserOrders" resultMap="userOrdersResultmap">
        SELECT
        u.`id`,
        u.`username`,
        u.`sex`,
        u.`birthday`,
        u.`address`,
        o.`id` oid,
        o.`user_id`,
        o.`number`,
        o.`createtime`,
        o.`note`
        FROM
        USER u
        LEFT JOIN orders o
        ON u.`id` = o.`user_id`
</select>



<resultMap id="userOrdersResultmap" type="user">
<!--两部分数据：用户+订单-->
<id column="id" property="id"/>
<result column="username" property="username"/>
<result column="sex" property="sex"/>
<result column="birthday" property="birthday"/>
<result column="address" property="address"/>

<!--一对多使用collection标签
        property：封装成对应的属性的属性名
        ofType：因为此时属性是一个list集合，collection已经表明了集合的意思，最重要的指定list里面的泛型类型，
        所以使用ofType
        -->
<collection property="ordersList" ofType="orders">
<id column="oid" property="id"/>
<result column="user_id" property="userId"/>
<result column="number" property="number"/>
<result column="createtime" property="createtime"/>
<result column="note" property="note"/>
</collection>
</resultMap>
```

 

## **多对多查询**

### **多对多关系分析** 

多对多是双向的一对多

### **实现Role到User的一对多**

l Mapper映射文件

```
<select id="queryRoleUsers" resultMap="roleUsersResultMap">
        SELECT
        r.rid,
        r.rname,
        r.rdesc,
        u.`username`,
        u.`address`
        FROM
        role r
        LEFT JOIN user_role ur
        ON r.`RID` = ur.`RID`
        LEFT JOIN USER u
        ON ur.`UID` = u.`id`
</select>

<!-- 多对多拆分成双向的一对多-->
<resultMap id="roleUsersResultMap" type="role">
<id column="rid" property="rid"/>
<result column="rname" property="rname"/>
<result column="rdesc" property="rdesc"/>

<collection property="userList" ofType="user">
<result column="username" property="username"/>
<result column="address" property="address"/>
</collection>
</resultMap>
```



### **实现User到Role的一对多**

下去练习下

# **[了解]扩展知识**

## **Tomcat配置JNDI数据源（了解）**

l 第一步：将数据库驱动程序(jar包)放到tomcat安装目录下的common\lib文件夹下

 ![1576478232380](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576478232380.png)

l 第二步：在Tomcat的conf/context.xml文件中进行jndi数据源服务配置

 ![1576478241706](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576478241706.png)

name：在JNDI中叫做目录名，等同于服务名，此处的jndi/mybatis是自定义的，往往以/连接前后字符串即可。

auth和type是固定的，其他都是数据库连接池的具体配置信息

 

l 第三步：在自己web项目的web.xml中引用Jndi数据源服务

 ![1576478271834](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576478271834.png)

l 第四步：在自己web项目的Mybatis配置文件中使用

 ![1576478278682](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576478278682.png)

配置data_source属性，指向你的数据源引用，java:comp/env/jndi/mybatis中红色部分是固定的，绿色部分是你自己定义的目录名（服务名）

# **工程准备**

使用Mapper动态代理的方式实现：

l 根据用户id查询用户

l 查询全部订单数据并关联查询出用户数据（一对一）

l 查询全部用户数据并关联查询出订单数据（一对多）

便于基于代码程序讲解Mybatis延迟加载和缓存。

 

# **[掌握]Mybatis延迟加载策略**

关联对象：数据库层次用户和订单是关联表，在对象层次订单对象和用户对象就叫做关联对象

## **什么是延迟加载？**

在使用关联对象的过程中

比如使用了Orders对象，但是我只是获取Orders的订单数据尚未获取user的数据，但是依然查询了user表（这不是延迟）

我orders.getNumber()你不要查user表的数据，当我orders.getUser()你再去查user表

 

关联查询的关联对象，在较多的场合中并不需要的时候，只在少部分场合需要关联的那个对象数据，可以考虑使用延迟加载

## **Mybatis中怎么实现延迟加载？**

步骤1：原来关联sql语句要拆分

1） 第一条sql查询基础表

2） 第二条sql根据基础表的条件查询关联表

步骤2：让两条sql自动产生联系，通过<association column="" select=""/> 或者<collection column="" select =""/>

l 全局延迟加载开关配置（一对一和一对多都需要开启）

 <!-- 开启延迟加载 --><setting name="lazyLoadingEnabled" value="true" /><!-- 关闭立即加载 --><setting name="aggressiveLazyLoading" value="false" /><!-- 设定tostring等方法延迟加载 --><setting name="lazyLoadTriggerMethods" value="true" />

l 一对一关联查询的延迟加载

```
<select id="queryOrdersWithUser" resultMap="ordersUserResultMap">
      SELECT
      o.`id`,
      o.`user_id`,
      o.`number`,
      o.`createtime`,
      o.`note`
      FROM
      orders o
</select>

<!--
      延迟加载分析
      1、原来关联查询的sql语句必然要拆分，如果不拆分，那肯定要执行关联查询，两个表用不用都要查
      2、sql=sql1+sql2拆分之后，要让这两个sql自动产生联系
      sql1:select id,user_id,number,createtime,note from orders
      sql2:select * from user u where id=user_id
      -->

<resultMap id="ordersUserResultMap" type="orders">
<id column="id" property="id"/>
<result column="user_id" property="userId"/>
<result column="number" property="number"/>
<result column="createtime" property="createtime"/>
<result column="note" property="note"/>
<association property="user" javaType="user" column="user_id" select="com.mybatis.mapper.UserMapper.queryUserByUserId">
</association>
</resultMap>

<select id="queryUserByUserId"  parameterType="int" resultType="user">
      select id,username,sex,birthday,address from user where id=#{user_id}
</select> 
```

l 一对多关联查询的延迟加载

```
<select id="queryUserWithOrders" resultMap="userOrdersResultMap">
      SELECT
      u.`id`,
      u.`username`,
      u.`sex`,
      u.`birthday`,
      u.`address`
      FROM
      USER u
</select>

<resultMap id="userOrdersResultMap" type="user">
<id column="id" property="id"/>
<result column="username" property="username"/>
<result column="sex" property="sex"/>
<result column="birthday" property="birthday"/>
<result column="address" property="address"/>

<collection property="ordersList" ofType="orders" column="id" select="com.mybatis.mapper.OrdersMapper.queryOrdersByUserId">
</collection>
</resultMap>

<!--拆分的第二条sql语句-->
<select id="queryOrdersByUserId" parameterType="int" resultType="orders">
      select o.id,o.user_id,o.number,o.createtime,o.note  from orders o  where o.user_id=#{id}
</select> 
```

# **[了解]Mybatis缓存**

## **Mybatis的一级缓存**

Mybatis的一级缓存是SqlSession级别的缓存，是默认开启的

 ![1576479425240](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576479425240.png)

**注意：**以下情形缓存会失效（被清理掉）：

n sqlSession关闭

n sqlSession提交事务：意味着可能是一个增删改的动作，数据表数据产生变化，那么这个时候Mybatis就会把这个sqlSession已有的一级缓存给清理掉

## **Mybatis的二级缓存**

l Mybatis二级缓存机制

 ![1576479451534](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576479451534.png)

l 怎么用Mybatis的二级缓存

n 开启二级缓存开关

第一个开关：需要在SqlMapConfig.xml中开启二级缓存**总开关**

第二个开关：需要在使用二级缓存的mapper.xml中开启（因为二级缓存是mapper级别的）

 

n Pojo实现序列化

 ![1576479486266](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576479486266.png)

l 结果

![1576479496298](https://github.com/lagouedu/Basic-document/blob/master/img-folder/MyBatis/1576479496298.png)

ratio：命中率的意思，两次都去查询了二级缓存，但是第一次的时候缓存中没有数据，所以没有命中，第二次查询的时候缓存中已经有数据了，所以命中率是0.5

l Mybatis二级缓存的清理时机：当这个mapper.xml中执行了非select语句的时候，整个Mapper的缓存全部清除掉。

注意：避免使用二级缓存，因为二级缓存带来的好处远远比不上他所隐藏的危害

n 如果你针对user表的sql操作并不是全部都在UserMapper.xml中，使用缓存的结果可能会不正确。

n 多表操作一定不能使用缓存

为什么不能？
首先不管多表操作写到那个Mapper.xml下，都会存在某个表不在这个Mapper下的情况。
例如两个表：role和user_role，如果我想查询出某个用户的全部角色role，就一定会涉及到多表的操作

```
<select id="selectUserRoles" resultType="UserRoleVO">
      select * from user_role a,role b where a.roleid = b.roleid and a.userid = #{userid}
</select>
```

像上面这个查询，你会写到那个xml中呢？？
不管是写到RoleMapper.xml还是UserRoleMapper.xml，或者是一个独立的XxxMapper.xml中。如果使用了二级缓存，都会导致上面这个查询结果可能不正确。
如果你正好修改了这个用户的角色，上面这个查询使用缓存的时候结果就是错的。

# **[掌握]Mybatis的注解开发**

效果：没有写sql语句的xml了

l 基础的crud（基础crud怎么使用注解）

l 关联查询延迟加载（怎么使用注解完成）

l 动态sql的注解方式

 

**原则：把原来xml里面的内容都对应的放到注解上即可**

 

**注解开发不需要引入额外的jar包，和原来一致即可**

**注解添加在接口类的方法上**

 

## **使用注解实现基本的CRUD**

```
@Insert("insert into user(username,sex,birthday,address) values (#{username},#{sex},#{birthday},#{address})")

/*回显mysql自增主键id 
  等同于原来我们xml中使用<selectKey/>，一一对应过来即可
* */
@SelectKey(statement = "select last_insert_id()",before = false,keyProperty = "id",resultType = Integer.class)
void saveUser(User user);

@Update("update user set username=#{username} where id=#{id}")
void updateUsernameById(User user);

@Delete("delete from user where id=#{id}")
void deleteUserById(Integer id);

@Select("select id,username,sex,birthday,address from user where id=#{id}")
User queryUserById(Integer id);

@Select("select id,username,sex,birthday,address from user")
List<User> queryUserList();


@Select("<script>select id,username,sex,birthday,address from user" +
        "<where>" +
        "<if test=\"sex != null and sex != ''\">" +
        " and sex=#{sex}" +
        "</if>" +
        "<if test=\"username != null and username != ''\">" +
        " and username like #{username}" +
        "</if>" +
        "</where></script>")
List<User> queryUserByWhere(User user);


@Select("<script>select id,username,sex,birthday,address from user" +
        "<foreach collection=\"list\" open=\" where id in(\" close=\")\" separator=\",\" item=\"item\">" +
        "#{item}" +
        "</foreach></script>")
List<User> queryUserByIdsList(List<Integer> ids); 
```

## **使用注解实现复杂关系映射开发**

### **使用注解实现一对一查询**

```
/*
 * 对应关系
 * @Results对应xml中的resultMap
 * @Result对应xml中的id/result
 * id：true/false，是否是主键，默认false
 * column：查询列名
 * property：pojo属性名
 * association：在注解中使用@Result+one=@One的方式去表达
 * fetchType：延迟加载类型，默认是延迟加载，eager：积极加载
 * */

@Results({
        @Result(id = true,column = "id",property = "id"),
        @Result(column = "user_id",property = "userId"),
        @Result(column = "number",property = "number"),
        @Result(column = "createtime",property = "createtime"),
        @Result(column = "note",property = "note"),
        @Result(property = "user",column = "user_id",javaType = User.class,one=@One(select = "com.mybatis.mapper.UserMapper.queryUserByUserid",fetchType = FetchType.LAZY))
})
List<Orders> queryOrdersUser();

@Select("select id,username,sex,birthday,address from user where id=#{user_id}")
User queryUserByUserid(Integer id); 
```

### **使用注解实现一对多查询**

l mapper接口

```
/*
 * 对应关系
 * @Results对应xml中的resultMap
 * @Result对应xml中的id/result
 * id：true/false，是否是主键，默认false
 * column：查询列名
 * property：pojo属性名
 * collection：在注解中使用@Result+many=@Many的方式去表达
 * xml中的ofType在注解中使用 javaType表达=List.class
 * fetchType：延迟加载类型，默认是延迟加载，eager：积极加载
 * */
@Select("select id,username,sex,birthday,address from user")
@Results({
        @Result(id = true,column = "id",property = "id"),
        @Result(column = "username",property = "username"),
        @Result(column="sex",property="sex"),
        @Result(column="birthday",property="birthday"),
        @Result(column="address",property="address"),
        @Result( property="ordersList",javaType = List.class,column="id",
                many = @Many(select="com.mybatis.mapper.OrdersMapper.queryOrdersByUserid",fetchType = FetchType.LAZY))
})
List<User> queryUserOrders();

@Select("select id,number from orders where user_id=#{userId}")
List<Orders> queryOrdersByUserid(Integer userId); 
```

 

 

 