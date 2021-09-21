# Mybatis


<!--more-->

## Mybatis

## 1、第一个Mybatis程序

### 导入maven依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
<!--      父工程  -->
    <groupId>cn.miaobuyi</groupId>
    <artifactId>ssm</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>

    <modules>
        <module>mybatis</module>
    </modules>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.8</version>
        </dependency>

        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.7</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13</version>
        </dependency>

    </dependencies>
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>


</project>
```

### 便携mybatis的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
<!--  每一个Mapper.XML都需要在Mybatis核心配置文件中注册！  -->
    <mappers>
        <mapper  resource="com/miaobuyi/mapper/UserMapper.xml"/>

    </mappers>
</configuration>
```



###  编写mybatis工具类

``` java
package com.miaobuyi.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

//sqlSessionFactory --> sqlSession
public class MybatisUtils {
    private  static SqlSessionFactory sqlSessionFactory;
    static {
       try {
           //使用Mybatis第一步：获取sqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = null;
            inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    //既然有了 ，顾名思义，我们就可以从中获得SqlSession的实例了。
    //SqlSession 完全包含了面向数据库执行SQL命令所需的所有方法
    public static SqlSession getSqlSeesion(){
        return  sqlSessionFactory.openSession();
    }
}

```

### 实体类

``` java
package com.miaobuyi.pojo;

public class User {
    private int id;
    private String name;
    private String pwd;

    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }
}

```

### Mapper 接口

``` java
package com.miaobuyi.mapper;


import com.miaobuyi.pojo.User;

import java.util.List;

public interface UserMapper {
    List<User> getUserList();
}

```

### XML文件里不要写中文注释要报错

接口实现类 由原来的UserMapperImpl转变成为一个Mapper配置文件

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace=绑定一个对应的Dao/Mappser接口-->
<mapper namespace="com.miaobuyi.mapper.UserMapper">
<!--selecte查询语句-->
    <select id="getUserList" resultType="com.miaobuyi.pojo.User">
        select * from mybatis.user
    </select>
</mapper>
```

### maven

maven由于他的约定大于配置，我们之后可能遇到我们配置文件，无法被到处或者生效的问题；解决方案pom.xml

``` xml
<!-- 在build中配置resource。来防止我们资源导出失败的问题   -->
<build>
    <resources>
        <resource>
            <directory>src/main/resource</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>

```

### 测试

```java
package com.miaobuyi.mapper;


import com.miaobuyi.pojo.User;
import com.miaobuyi.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

public class UserMapperTest {

    @Test
    public void test(){
        //第一步获取SqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //执行sql  方式1：getMapper
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        List<User> userList = userMapper.getUserList();

        //方式2 老方式
        //userList =  sqlSession.selectList("com.miaobuyi.mapper.UserMapper.getUserList");
        for (User user :userList){
            System.out.println(user);
        }

        //关闭SqlSession
        sqlSession.close();
    }
}

```

## 2、CRUD

### 1、namesapce

namespace中的包名要和Dao/mapper接口中的包名一致！

### 2、select

选择，查询一句：

* id：就是对应namespace中的方法名
* resultType：sql语句执行的返回值
* parameterTpey：参数类型

1. 编写接口

   ```java
   //根据ID查询用户
       User getUserById(int id);
   ```

   

2. 编写对应mapper中的sql语句

   ```xml
       <select id="getUserById" resultType="com.miaobuyi.pojo.User" parameterType="int">
           select * from mybatis.user where id = #{id}
       </select>
   ```

   

3. 测试

   ``` java
    @Test
       public void getUserById(){
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           User user = mapper.getUserById(1);
           System.out.println(user);
           sqlSession.close();
       }
   ```

   

### 3、insert

```xml
<insert id="addUser" parameterType="com.miaobuyi.pojo.User">
        insert  into mybatis.user (id,name,pwd)values (#{id},#{name},#{pwd})
    </insert>
```



### 4、update

``` xml
<update id="updateUser" parameterType="com.miaobuyi.pojo.User">
        update mybatis.user set name = #{name},pwd=#{pwd} where id=#{id};
    </update>
```



### 5、delete

``` xml
    <delete id="deleteUser" parameterType="int">
        delete from mybatis.user where id=#{id}
    </delete>
```

注意：增删改需要提交事务

```java
sqlSessionFactory.openSession(true);//工具类中  自动开启提交事务
```

``` java
sqlSession.commit();
```

### 6、分析错误

* 标签不要匹配错误

* resource绑定mapper，需要使用路径

* 程序配置文件必须符合规范

* NullPointerException，没有注册到资源

* 输出的xml文件中存在中文乱码问题（最好不要写注释）

* maven资源没有导出问题

### 7、万能map

假设，我们的实体类，或者数据库中的表，字段或者参数过多，我们应当考虑使用map！

``` java
    //万能map
    User addUser2(Map<String,Object> map);
```

```xml
<insert id="addUser2" parameterType="map">
        insert  into mybatis.user (id,name,pwd)values (#{userid},#{uerName},#{password})
    </insert>
```

```java
 @Test
    public void addUser2(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        Map<String,Object> map = new HashMap<String, Object>();
        map.put("userid","2");
        map.put("userName","2");
        map.put("password","2");
        mapper.addUser2(map);
        sqlSession.close();
    }
```

Map传递参数时，在sql中直接取出key即可

对象传递参数，直接在sql中取对象属性即可

只有一个基本类型参数的情况下，可以直接在sql中取到

多个参数用Map，或者**注解**

### 8、思考题

模糊查询怎么写？

1. java代码执行的时候，传递通配符% %

   ``` java
   List<User> userList = mapper.getUserLike("%李%");
   ```

2. 在sql拼接中使用通配符！

   ```sql
   select * from mybatis.user where name like "%"#{value}"%"
   ```

## 3、配置解析

### 1、核心配置文件

* mybatis-config.xml

* Mybatis的配置文件包含了会深深影响Mybatis行为的设置和属性信息

  ```xml
   configuration（配置）
   properties（属性）
   settings（设置）
   typeAliases（类型别名）
   typeHandlers（类型处理器）
   objectFactory（对象工厂）
   plugins（插件）
   environments（环境配置）
   environment（环境变量）
   transactionManager（事务管理器）
   dataSource（数据源）
   databaseIdProvider（数据库厂商标识）
   mappers（映射器）
  ```

### 2、环境配置（environments）

MyBatis 可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

学会使用多套配置运行环境

mybatis默认的食物管理器就是JDBC，连接池：POOLED

### 3、属性（properties）

我们可以通过properties属性来实现引用配置文件

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。

db.properties

编写一个配置文件

``` properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8
username=root
password=123456
```

在核心配置文件中引用

```xml
<properties resource="db.properties"/>
```

* 可以直接引入外部文件
* 可以在其中增加一些配置属性
* 如果两个文件有同一个字段，先读取properties元素体内指定的属性，然后根据properties元素中resource属性读取类路径下属性文件，或根据url属性指定的路径读取属性文件，并覆盖之前读取过得同名属性，最后读取作为方法参数传递的属性，并覆盖之前读取过的同名属性

### 4、类型别名(typeAliases)

* 类型别名是java类型设置一个短的名字
* 它仅用于 XML 配置，意在降低冗余的全限定类名书写

``` xml
    <typeAliases>
        <typeAlias type="com.miaobuyi.pojo.User" alias="User"/>
    </typeAliases>
```

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean

扫描实体类的包，它的默认别名就为这个类的类名，首字母小写！

```xml
    <typeAliases>
       <package name="com.miaobuyi.pojo"/>
    </typeAliases>
```

在实体类比较少的时候使用第一种方式

如果实体类十分多，建议使用第二种

第一种可以DIY别名，第二种不行，如果非要改，需要在实体上增加注解

```java
@Alias("user")
public class User {}
```

### 5、设置

这是MyBatis中极为重要的调整设置，它们会改变MyBatis的运行时行为。

![image-20210714190420538](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210714190420538.png)

![image-20210714190451330](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210714190451330.png)

#### 6、其他设置

 

* typeHandlers（类型处理器）

* objectFactory（对象工厂）
*  plugins（插件）
  * mybatis-generator-core
  * mybatis-plus
  * 通用mapper

### 7、映射器（mappers）

MapperRegistry：注册绑定我们的Mapper文件；

方式一：【推荐使用】

```xml
<!--  每一个Mapper.XML都需要在Mybatis核心配置文件中注册！  -->
    <mappers>
        <mapper  resource="com/miaobuyi/mapper/UserMapper.xml"/>
        <mapper class="com.miaobuyi.mapper.UserMapper"/>
    </mappers>
```

方式二：使用class文件绑定注册

```xml
<mappers>
    <mapper class="com.miaobuyi.mapper.UserMapper"/>
</mappers>
```

注意点：

* 接口和他的Mapper配置文件必须同名！
* 接口和他的Mapper配置文件必须在同一个包下！

方式三：

``` xml
    <mappers>
        <package name="com.miaobuyi.mapper"/>
    </mappers>

```

* 接口和他的Mapper配置文件必须同名！（UserMapper接口和UserMapper.xml）
* 接口和他的Mapper配置文件必须在同一个包下！

### 8、生命周期和作用域

作用域和生命周期是至关重要的，因为错误的使用会导致非常严重的**并发问题**。

**SqlSessionFactoryBuilder**：

* 一旦创建了 SqlSessionFactory，就不再需要它了
* 局部变量

**SqlSessionFactory**：

* 说白了就是可以想象为：数据链接池
* SqlSessionFactory一旦被创建就应该在应用的运行期间一直存在，**没有任何理由丢弃它或重新创建另一个实例**
* 因此 SqlSessionFactory 的最佳作用域是应用作用域。 
* 有很多方法可以做到，最简单的就是使用**单例模式**或者静态单例模式

**SqlSession**

* 链接到连接池的一个请求！
* SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域
* 用完之后需要赶紧关闭，否则资源被占用！

![image-20210714194447645](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210714194447645.png)

这里面的每一个Mapper，就代表一个具体的业务！

## 4、解决属性名和字段名不一致的问题

### 1、问题

数据库中的字段

![image-20210714194722061](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210714194722061.png)

新建一个项目，拷贝之前的，测试实体类字段不一致的情况

```java
public class User {
    private int id;
    private String name;
    private String password;
}
```

测试出现问题

![image-20210714195944486](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210714195944486.png)

解决办法

* 起别名

    ```sql
    
    //select * from mybatis.user where id = #{id}
    //类型处理器
    //select id,name,pwd from mybatis.user where id = #{id}```
    ```
    
    

### 2、resultMap

结果集映射

```
id name pwd
id name password
```

``` xml
    <!--结果集映射-->
    <resultMap id="UserMap" type="user">
            <!--column数据库中的字段，property实体类中的属性-->
        <result column="id" property="id"></result>
        <result column="name" property="name"></result>
        <result column="pwd" property="password"></result>
     </resultMap>

    <select id="getUserById" resultMap="UserMap">
        select * from mybatis.user where id = #{id}
    </select>
```

* `resultMap` 元素是 MyBatis 中最重要最强大的元素
* `ResultMap` 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。
* `ResultMap`的优秀之处,虽然你已经对他相当了解了，但是根本就不需要显式地配置它们。

## 5、日志

### 1、日志工程

如果一个数据库操作，出现了异常，我们需要配错，日志就是最好的助手

曾经：sout，debug

现在：日志工厂！

![image-20210715124954090](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210715124954090.png)



* SLF4J 
*  LOG4J 【掌握】
*  LOG4J2 
*  JDK_LOGGING 
*  COMMONS_LOGGING 
*  STDOUT_LOGGING 【掌握】
*  NO_LOGGING



在mybatis中具体使用哪一个日志实现，在设置中设定

**STDOUT_LOGGING标准日志输出**

```xml
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
```



![image-20210715125831931](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210715125831931.png)

### 2、LOG4J 

什么是LOG4J ？

* Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件
* 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。
* 通过一个[配置文件](https://baike.baidu.com/item/配置文件/286550)来灵活地进行配置，而不需要修改应用的代码。



1. 先导入LOG4J 的包、

   ``` xml
   <!-- https://mvnrepository.com/artifact/log4j/log4j -->
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   ```

2. log4j.properties

   ``` properties
   #将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
   log4j.rootLogger=DEBUG,console,file
   
   #控制台输出的相关设置
   log4j.appender.console = org.apache.log4j.ConsoleAppender
   log4j.appender.console.Target = System.out
   log4j.appender.console.Threshold=DEBUG
   log4j.appender.console.layout = org.apache.log4j.PatternLayout
   log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
   
   #文件输出的相关设置
   log4j.appender.file = org.apache.log4j.RollingFileAppender
   log4j.appender.file.File=./log/miaobuyi.log
   log4j.appender.file.MaxFileSize=10mb
   log4j.appender.file.Threshold=DEBUG
   log4j.appender.file.layout=org.apache.log4j.PatternLayout
   log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n
   
   #日志输出级别
   log4j.logger.org.mybatis=DEBUG
   log4j.logger.java.sql=DEBUG
   log4j.logger.java.sql.Statement=DEBUG
   log4j.logger.java.sql.ResultSet=DEBUG
   log4j.logger.java.sql.PreparedStatement=DEBUG
   ```

3. 配置log4j为日志的实现

   ``` xml
       <settings>
           <setting name="logImpl" value="LOG4J"/>
       </settings>
   ```

4. log4j的使用！直接运行刚才的查询

   ![image-20210715132513047](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210715132513047.png)

   ![image-20210715132537236](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210715132537236.png)



**简单使用**

1. 在要使用log4j类中，导入包 import org.apache.log4j.Logger;

2. 日志对象，参数为当前类的class

   ``` java
       static Logger logger = Logger.getLogger(UserMapperTest.class);
   ```

3. 日志级别

   ``` java
           logger.info("info:进入了testLog4j");
           logger.debug("debug:进入了testLog4j");
           logger.error("error:进入了testLog4j");
   ```

## 6、分页

**思考：为什么要分页**

* 减少以数据的处理量



### 1、使用Limit分页

``` sql
语法：SELECT * from user limit startIndex,pageSize
select * from mybatis.user limit 3; #[0,n]
```



使用Mybatis分页，核心SQL

1. 接口

   ``` xml
       List<User> getUserByLimit(Map<String,Integer> map);
   ```

2. Mapper.xml

   ```xml
   <select id="getUserByLimit" resultMap="UserMap" parameterType="map">
       select * from mybatis.user limit #{startIndex},#{pageSize}
   </select>
   ```

3. 测试

   ```java
   @Test
   public void getUserByLimit(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       HashMap<String, Integer> map = new HashMap<String, Integer>();
       map.put("startIndex",0);
       map.put("pageSize",2);
       List<User> userList = mapper.getUserByLimit(map);
       for (User user:userList){
           System.out.println(user);
       }
       sqlSession.close();
   }
   ```



### 2、RowBounds【了解】

不再使用SQL实现分页

1. 接口

   ```java
   List<User> getUserByRowBounds();
   ```

2. mapper.xml

   ```xml
   <select id="getUserByRowBounds" resultMap="UserMap">
       select * from mybatis.user
   </select>
   ```

3. 测试

   ```java
   @Test
   public void getUserByRowBounds(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       //RowBounds实现
       RowBounds rowBounds = new RowBounds(1, 2);
   
       //通过java代码层面实现分页
       List<User> userList = sqlSession.selectList("com.miaobuyi.mapper.UserMapper.getUserByRowBounds",null,rowBounds);
       for (User user:userList){
           System.out.println(user);
       }
       sqlSession.close();
   }
   ```

### 3、插件分页【了解】

![image-20210715165613496](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210715165613496.png)



## 7、使用注解开发

### 1、面向接口编程

大家之前都学过面向对象编程，也学习过接口，但在真正的开发中，很多时候我们会选择面向接口编程

**-根本原因∶解耦，可拓展，提高复用，分层开发中，上层不用管具体的实现，大家都遵守共同的标准，使得开发变得容易，规范性更好**

- 在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下.各个对象内部是如何实现自己的,对系统设计人员来讲就不那么重要了;

- 而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。

**关于接口的理解**

- 接口从更深层次的理解，应是定义(规范，约束）与实现(名实分离的原则）的分离。

- 接口的本身反映了系统设计人员对系统的抽象理解。

- 接口应有两类:
  - 第一类是对一个个体的抽象，它可对应为一个抽象体(abstract class);
  - 第二类是对一个个体某一方面的抽象，即形成一个抽象面( interface) ;
- 一个体有可能有多个抽象面。抽象体与抽象面是有区别的。

**三个面向区别**

- 面向对象是指，我们考虑问题时，以对象为单位,考虑它的属性及方法.

- 面向过程是指，我们考虑问题时，以一个具体的流程(事务过程)为单位，考虑它的实现.

- 接口设计与非接口设计是针对复用技术而言的，与面向对象(过程)不是一一个问题.更多的体现就是对系统整体的

### 2、使用注解开发

1. 注解在接口上实现

   ```java
   @Select("select * from user")
   List<User> getUsers();
   ```

2. 需要在核心配置文件中绑定接口

   ```xml
   <!--    绑定接口-->
   <mappers>
       <mapper class="com.miaobuyi.mapper.UserMapper"/>
   </mappers>
   ```

3. 测试

   ```java
   @Test
   public void test(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       List<User> userList = mapper.getUsers();
       for(User user:userList){
           System.out.println(user);
       }
       sqlSession.close();
   }
   ```



本质：反射机制实现

底层：动态代理！

![image-20210715175854323](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210715175854323.png) 

**Mybatis详细的执行流程！**



## 8、Lombok

使用步骤

1. 在IDEA中安装Lombok

2. 在项目中导入Lombok的jar包

   ```xml
   <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.20</version>
   </dependency>
   
   ```

3. 

   ```java
   @Getter and @Setter
   @FieldNameConstants
   @ToString
   @EqualsAndHashCode
   @AllArgsConstructor, @RequiredArgsConstructor and @NoArgsConstructor
   @Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog
   @Data
   @Builder
   @SuperBuilder
   @Singular
   @Delegate
   @Value
   @Accessors
   @Wither
   @With
   @SneakyThrows
   @val
   @var
   experimental @var
   @UtilityClass
   @ExtensionMethod (Experimental, activate manually in plugin settings)
   ```

   ``` java
   @Data:无参构造，get，set，toString，hashcode，equals
   @AllArgsConstructor
   @NoArgsConstructor
   @FieldNameConstants
   @ToString
   ```

   

   

## 9、多对一处理

多对一：

![image-20210716171244575](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210716171244575.png)

* 多个学生，对应一个老师
* 对于学生这边而言，关联。。对个学生，关联一个老师
* 对于老师而言，集合，

![image-20210716174404362](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210716174404362.png)

绿色：1   蓝色：∞



SQL：

```sql
CREATE TABLE `teacher` (
                           `id` INT(10) NOT NULL,
                           `name` VARCHAR(30) DEFAULT NULL,
                           PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO teacher(`id`, `name`) VALUES (1, '秦老师');

CREATE TABLE `student` (
                           `id` INT(10) NOT NULL,
                           `name` VARCHAR(30) DEFAULT NULL,
                           `tid` INT(10) DEFAULT NULL,
                           PRIMARY KEY (`id`),
                           KEY `fktid` (`tid`),
                           CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO `student` (`id`, `name`, `tid`) VALUES (1, '小明', 1);
INSERT INTO `student` (`id`, `name`, `tid`) VALUES (2, '小红', 1);
INSERT INTO `student` (`id`, `name`, `tid`) VALUES (3, '小张''', 1);
INSERT INTO `student` (`id`, `name`, `tid`) VALUES (4, '小李', 1);
INSERT INTO `student` (`id`, `name`, `tid`) VALUES (5, '小王', 1);
```



### 测试环境搭建

1. 导入Lombok
2. 新建实体类Teacher，Student
3. 建立Mapper接口
4. 建立Mapper.XML文件
5. 在核心配置文件中绑定注册我们的Mapper接口或者文件！【方式很多，随心选】
6. 测试查询是否能能够成功



### 按照查询嵌套处理

``` xml
<!--
        思路:
            1.查询所有的学生信息
            2.根据查询出来的学生的tid，寻找对应的老师! 子查询
    -->
<select id="getStudent" resultmap="studentTeacher">
    select * from student
    </se1ect>

<resultMap id="StudentTeacher" type="student">
    <resu1t property="id" column="id"/>
    <resu1t property="name" column="name"/>

    <!--复杂的属性，我们需要单独处理对象:association集合:collection -->
    <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
    </resu1tMap>

<select id="getTeacher" resultType="Teacher">
    select * from teacher where id = #{id}
</select>

```



### 按照结果嵌套处理

``` xml
<select id="getStudent2" resultMap="StudentTeacher2">
    select s.id sid,s.name sname,t.name tname
    from student s,teacher t
    where s.tid = t.id

</select>

<resultMap id="StudentTeacher2" type="Student">
    <result property="id" column="id" />
    <result property="name" column="name"/>
    <association property="teacher" javaType="Teacher">
        <result property="name" column="tname"></result>
    </association>
</resultMap>
```



回顾Mysql多对一查询方式

* 子查询
* 联表查询



## 10、一对多处理

比如：一个老师拥有多个学生！

对于老师而言就是一对多的关系！

### 环境搭建

1. 环境搭建和刚才一样

   **实体类**

   ```java
   @Data
   public class Student {
       private  int id;
       private String name;
   
       private int tid;
   }
   
   ```

   ```java
   @Data
   public class Teacher {
       private int id;
       private String name;
   
       //一个老师拥有多个学生
       private List<Student> students;
   
   }
   ```



### 按照过嵌套处理

```xml
<select id="getTeacher" resultMap="TeacherStudent">
    select s.id sid ,s.name sname,t.name tname,t.id tid
    from student s,teacher t
    where s.tid = t.id and t.id = #{tid}
</select>
<resultMap id="TeacherStudent" type="Teacher">
    <result property="id" column="tid"/>
    <result property="name" column="tname"/>
    <collection property="students" ofType="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <result property="tid" column="tid"/>
    </collection>
</resultMap>
```





### 按查询嵌套处理

```xml
<select id="getTeacher2" resultMap="TeacherStudent2">
    select * from mybatis.teacher where id = #{tid}
</select>
<resultMap id="TeacherStudent2" type="Teacher">
    <collection property="students" javaType="ArrayList" ofType="Student" select="getStudentByTeacherId" column="id"/>
</resultMap>

<select id="getStudentByTeacherId" resultType="Student">
    select * from mybatis.student where tid=#{tid}
</select>
```



小结

1. 关联 - association【多对一】
2. 集合 - collection 【一对多】 
3. javaType & ofType
   1. javaType	用来指定实体类中的属性类型
   2. ofType   用来指定映射到List或者集合中的pojo类型中，泛型中的约束类型



注意点：

* 保证SQL的可读性，尽量保证通俗易懂
* 注意一对多和多对一中，属性名和字段的问题
* 如果问题不好排查错误，可以使用日志。建议使用log4j

面试高频

* Mysql引擎
* InnoDB一层原理
* 索引
* 索引优化！



## 11、动态SQL

**什么是动态SQL：动态SQL就是根据不同条件生成不同的SQL语句**

利用动态SQL这一特性可以彻底摆脱这种痛苦。

``` 
动态SQL元素和JSTL或基于类似XML的文本处理器相似。在MyBatis 之前的版本中，有很多元素需要花时间
了解。MyBatis 3大大精简了元素种类，现在只需学习原来一半的元素便可。MyBatis采用功能强大的基于OGNL
的表达式来淘汰其它大部分元素。
if
choose (when， otherwise)
trim (where， set)
foreach

```



### 搭建环境

``` sql
CREATE TABLE `blog`(
    `id` VARCHAR(50) NOT NULL COMMENT '博客id',
    `title` varchar(100) NOT NULL COMMENT '博客标题',
    `author` varchar(30) NOT NULL COMMENT '博客作者',
    `create_time` datetime NOT NULL COMMENT '创建时间',
    `views` int(30) NOT NULL COMMENT '浏览量'
)ENGINE=INNODB DEFAULT CHARSET=utf8
```



创建一个基础工程

1. 导包

2. 编写配置文件

3. 编写实体类

   ```java
   @Data
   public class Blog {
       private int id;
       private String title;
       private String author;
       private Date creatTime;
       private int views;
   }
   ```

4. 编写实体类对应的Mapper接口和Mapper.XML文件

   

### IF

```xml
<select id="queryBlogIf" parameterType="map" resultType="Blog">
    select * from mybatis.blog where 1=1
    <if test="title != null">
        and title = #{title}
    </if>
    <if test="author != null">
        and author = #{author}
    </if>
</select>
```



### choose (when， otherwise)

```xml
<select id="queryBlogChoose" parameterType="map" resultType="Blog">
    select * from mybatis.blog
    <where>
        <choose>
            <when test="title != null">
                title = #{title}
            </when>
            <when test="title != null">
                and authot = #{author}
            </when>
            <otherwise>
                and views = #{views}
            </otherwise>
        </choose>
    </where>
</select>
```







### trim (where， set)【trim可以自定义】

```xml
    <update id="updateBlog" parameterType="map" >
        update mybatis.blog
        <set>
            <if test="title != null">
                title = #{title},
            </if>
            <if test="author != null">
                author = #{author},
            </if>
        </set>
        where id = #{id}
    </update>
```

```
<trim prefix="" prefixOverrides="" suffix="" suffixOverrides="">
    
</trim>
```



**所谓动态SQL，本质还是SQL语句，只是我们可以再SQL层面去执行一些逻辑代码**

if 

where set  choose  when



### SQL片段

有的时候，我们可能会讲一些功能的部分抽取出来，方便复用

1. 使用SQL标签抽取的公共部分

```xml
<sql id="if-title-author">
    <if test="title != null">
        title = #{title}
    </if>
    <if test="author != null">
        and author = #{author}
    </if>
</sql>

```

2. 在需要使用的地方使用include标签引用即可

```xml
<select id="queryBlogIf" parameterType="map" resultType="Blog">
    select * from mybatis.blog
    <where>
       <include refid="if-title-author"></include>
    </where>
</select>
```



### Foreach

```sql
select * from user where 1=1 and 
<foreach item="id" index="index" collection="ids"
	open="(" separator = "or"  close=")"
	#{id}
</foreach>


(id = 1 or id=2 or id =3)





```



![image-20210717161103126](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210717161103126.png)

```xml
<select id="queryBlogForeach" parameterType="map" resultType="Blog">
    select * from mybatis.blog

    <where>
        <foreach collection="ids" item="id" open="and (" close=")" separator="or">
            id = #{id}
        </foreach>
    </where>
</select>
```



 **动态SQL就是在拼接SQL语句，我们只要保证SQL的正确性，按照SQL的格式，去排列组合就可以了**

建议

* 先在Mysql中写出完整的SQL，在对应的去修改成为我们的动态SQL实现通用





## 12、缓存

### 1、简介

``` 
查询 ：链接数据库 ，耗资源
	一次查询的结果，给他暂存一个可以直接取到的地方！-->内存 ：缓存
	
我们再次查询相同的数据的时候，直接走缓存，就不用走数据库了
```



1. 什么是缓存[ Cache ]?
   * 存在内存中的临时数据。
   * 将用户经常查询的数据放在缓存(内存)中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询，从缓存中查询，从而提高查询效率,解决了高并发系统的性能问题。
2. 为什么使用缓存?
   * 减少和数据库的交互次数,减少系统开销,提高系统效率。
3. 什么样的数据能使用缓存?
   * 经常查询并且不经常改变的数据。【可以使用缓存】





### 2、Mybatis缓存

* MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效
  率。
* MyBatis系统中默认定义了两级缓存: **一级缓存**和**二级缓存**
  * 默认情况下，只有一-级缓存开启。 (SqlSession级别的缓存， 也称为本地缓存)
  * 二级缓存需要手动开启和配置， 他是基于namespace级别的缓存。
  * 为了提高扩展性, MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存



### 3、一级缓存

* 一级缓存也叫本地缓存: SqlSession
  * 与数据库同一次会话期间查询到的数据会放在本地缓存中。
  * 以后如果需要获取相同的数据，直接从缓存中拿，没必须再去查询数据库;

测试步骤：

1. 开启日志

2. 测试在一个Session中查询两次相同的记录

3. 查看日志输出

   ![image-20210717175732648](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210717175732648.png)



缓存失效的情况

1. 查询不同的东西

2. 增删改操作，可能会改变原来的数据，所以必定会刷新缓存！

   ![image-20210717180544621](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210717180544621.png)

3. 查询不同的Mapper.xml

4. 手动清理缓存！

   ```java
   @Test
   public void test(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       User user = mapper.queryUserById(1);
       System.out.println(user);
   
       sqlSession.clearCache();//手动清理缓存
       System.out.println("=========================");
   
       User user2 = mapper.queryUserById(1);
       System.out.println(user2);
   
   
   
       System.out.println(user==user2);
       sqlSession.close();
   
   }
   ```



小结：一级缓存默认是开启的，只有在一次SqlSession中有效，也就是拿到链接到关闭连接这个区间段！

一级缓存就是一个Map



### 4、 二级缓存

* 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存
* 基于namespace级别的缓存， 一个名称空间，对应一 个二级缓存
* 工作机制
  * 个会话查询一条数据，这个数据就会被放在当前会话的- -级缓存中
  * 如果当前会话关闭了，这个会话对应的一级缓存就没了;但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中
  * 新的会话查询信息，就可以从二级缓存中获取内容;
  * 不同的mapper查出的数据会放在自己对应的缓存(map)中

步骤：

1. 开启全局缓存

   ```xml
   <!--        显示的开启全局缓存-->
           <setting name="cacheEnabled" value="true"/>xxxxxxxxxx2 1<!--        显示的开启全局缓存-->2        <setting name="cacheEnabled" value="true"/><setting name="cacheEnabled" value="true"/>
   ```

2.  再要使用deMapper中开启

   ```xml
   <!--        在当前xml中使用缓存-->
   <cache/>
   ```

   也可以自定义参数

   ```xml
   <cache eviction="FIFO"
          flushInterval="60000"
          size="512"
          readOnly="true"/>
   ```

3.  测试

   1. 问题：我们需要将实体类序列化！否则就会报错！

      ``` java
      Caused by: java. io. NotSerializableExcepti on:com.miaobuyi.pojo.user
      ```

      

      ``` java
      @Data
      public class User implements Serializable {
          private int id;
          private String name;
          private String pwd;
      }
      
      ```



小结：

* 只要开启了二级缓存，在同一个Mapper下就有效
* 所有的数据都会先放在一级缓存中
* 只有当回话提交或者关闭的时候，才会提交到二级缓存中！



### 5、缓存原理

![image-20210717191347109](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210717191347109.png)



### 6、自定义缓存-ehcache





Redis数据库来做缓存！ K-V


