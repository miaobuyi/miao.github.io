# Spring


<!--more-->

## 1、Spring

### 1、简介

* 2002 ，首次推出了Spring框架的雏形：**interface21**框架

* 2004-3-24正式发布1.0版本，Spring框架以interface21框架为基础，经过重新设计，并不对丰富其内涵
* Rod Johnson，Spring框架的创始人
* spring的理念：使现有的技术更加容易使用，本身是一个大杂烩，整合了现有的技术框架



* SSH ： Struct + Spring + Hibernate！
* SSM ： SpringMVC + Spring + Mybatis！



maven

``` xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.9</version>
</dependency>

```



### 2.优点

* Spring是一个开源的免费的框架（容器）！
* Spring是一个轻量级，非入侵式的容器！
* 控制翻转（IOC），面向切面编程（AOP）
* 支持事务的处理，对框架整合的支持



总结一句话：Spring就是一个轻量级的控制反转（IOC)和面向切面(AOP)的框架

### 3、组成

![image-20210719152443040](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210719152443040.png)



### 4、拓展

在Spring的官网有这个介绍：现代化的java开发！说白了就是基于Spring开发

![image-20210719152719225](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210719152719225.png)



* Srping Boot
  * 一个快速开发的脚手架
  * 基于SpringBoot可以快速的开发单个微服务
  * 约定大于配置！
* Spring Cloud
  * SpringCloud是基于SpringBoot实现的。



因为现在大多数公司都在使用SpringBoot进行快速开发，学习SpringBoot的前提，需要完全掌握Srping和SpringMVC



**弊端：发展了太久之后，违背了原来的理念！配置十分繁琐，人称“配置地狱！”**



## 2、IOC理论推导

1. UserDao接口
2. UserDaoImpl实现类
3. USerService业务接口
4. UserServiceUmpl业务实现类



在我们之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改源代码！如果程序代码量十分大，修改一次的代码代价十分昂贵！

![image-20210719165232920](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210719165232920.png)

我们使用一个Set接口实现

```java
private UserMapper userMapper = new UserMapperImpl();

//利用set进行动态实现值得注入
public void setUserMapper(UserMapper userMapper) {
    this.userMapper = userMapper; 
}
```

* 之前，程序是主动创建对象！控制权在程序猿手上
* 使用set注入后，程序不在具有主动性，而是变成了被动的接受对象



这种思想，从本质上解决了问题，我们程序猿不再去管理对象的创建。系统的耦合性大大减低，可以更加专注地在业务的实现上！这是IOC的原型！ 

![image-20210719165243411](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210719165243411.png)

### IOC本质

**控制反转loC(Inversion of Control),是-种设计思想，DI(依赖注入)是实现IoC的一种方法**，也有人认为DI只是IoC的另一种说法。 没有IoC的程序中,我们使用面向对象编程,对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是:获得依赖对象的方式反转了。

采用XML方式配置Bean的时候，Bean的定 义信息是和实现分离的，而采用注解的方式可以把两者合为一体,Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述(XML或注解) 并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入(Dependency Injection,DI)。**



**IOC实际上是对象由spring创建，管理，装配**



## 3、HelloSpring

### 1.导入Spring相关的Jar包

注：Spring需要导入commons-logging进行日志记录，我们利用maven，它会自动下载对应的依赖项

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.9</version>
</dependency>
```

### 2、编写相关代码

1. 编写一个Hello实体类

```java
package com.miaobuyi.pojo;

public class Hello {
    private String str;

    public String getStr() {
        return str;
    }

    public void setStr(String str) {
        this.str = str;
    }

    @Override
    public String toString() {
        return "Hello{" +
                "str='" + str + '\'' +
                '}';
    }
}
```

2. 在xml中注册

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
   <!--    使用Spring来创建对洗那个，在Spring这些都成为Bean
   
           类型 变量名 = new 类型();
           Hello hello = new Hello();
           bean = 对象    new Hello();
   
   
           id = 变量名
           class = new 的对象
           property 相当于给对象中的属性设置一个值
   -->
       <bean id="hello" class="com.miaobuyi.pojo.Hello">
           <property name="str" value="Spring"/>
       </bean>
   
   </beans>
   ```

3. 测试类

   ```java
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class MyTest {
       public static void main(String[] args) {
   
           //获取Spring的上下文对象！
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           //我们的对象都在Spring中管理了，我们要使用，直接去里面取出来就可以了！
           Object hello = context.getBean("hello");
           System.out.println(hello.toString());
       }
   }
   ```

思考问题?

* Hello对象是谁创建的?
  * hello对象是由Spring创建的
* Hello 对象的属性是怎么设置的?
  * hello对象的属性是由Spring容器设置的，



这个过程就叫控制反转 :

控制：谁来控制对象的创建,传统应用程序的对象是由程序本身控制创建的,使用Spring后,对象是由Spring来创建的.
反转：程序本身不创建对象,而变成被动的接收对象.
I依赖注入：就是利用set方法来进行注入的.
IOC是一种编程思想,由主动的编程变成被动的接收.
可以通过newClassPathXmlApplicationContext去浏览一下底层源码 .
**OK,到了现在,我们彻底不用再程序中去改动了,要实现不同的操作,只需要在xml配置文件中进行修改,所谓的loC,一句话搞定:对象由Spring来创建,管理,装配!**



## 4、IOC创建对象的方式



1. 使用无参构造创建对象，默认

2. 假设我们要使用有参构造创建对象

   1. 下标赋值

      ```xml
      <bean id="user" class="com.miaobuyi.pojo.User" >
          <constructor-arg index="0" value="喵不易学java"/>
      </bean>
      ```

   2. 类型

      ```xml
      <bean id="user" class="com.miaobuyi.pojo.User">
          <constructor-arg type="java.lang.String" value="miaobuyi"/>
      </bean>
      ```

   3. 参数名

      ```xml
      <bean id="user" class="com.miaobuyi.pojo.User">  
          <constructor-arg name="name" value="喵不易"/>
      </bean>
      ```

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了



## 5、Spring配置



### 1、别名

```xml
<!--    别名，如果添加了别名，我们也可以使用别名获取到这个对象-->
    <alias name="user" alias="usernew"/>
```

### 2、Bean的配置

```xml
<!--
    id：bean的唯一标识符，也就是相当于我们学的对象名
    class：bean对选哪个所对应的全限定名：包名+类型
    name：也是别名，而且name更高级 可以同时取多个别名
-->
    <bean id="user" class="com.miaobuyi.pojo.User" name="userNew,u2" >
        <constructor-arg name="name" value="喵不易"/>
    </bean>
```

### 3、import

这个import，一般用于团队开发使用，他可以将多个配置文件导入合并为一个

假设，现在项目中有多个人开发，这三个人负责不同的类开发，不同的类需要注册在不同的bean中，我们可以利用import将所有人beans.xml合并为一个总的！

* 张三

* 李四

* 王五

* applicationContext.xml

  ```xml
  <import resource="beans.xml"/>
  ```

  

使用的时候直接使用总的配置就可以了





## 6、DI依赖注入



### 1、构造器注入

 前面有



### 2、Set方式注入【重点 】

* 依赖注入：Set注入
  * 依赖：bean对象的创建依赖于容器！
  * 注入：bean对象中的所有属性，由容器注入



【环境搭建】

1. 复杂类型

   ```java
   package com.miaobuyi.pojo;
   
   public class Address {
       private String address;
   
       public String getAddress() {
           return address;
       }
   
       public void setAddress(String address) {
           this.address = address;
       }
   }
   ```

2. 真实测试对象

   ```java
   public class Student {
       private String name;
       private Address address ;
       private String[] books ;
       private List<String> hobbys;
       private Map<String, String> card;
       private Set<String> games;
       private String wife;
       private Properties info;
   }
   ```

3. beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="student" class="com.miaobuyi.pojo.Student">
   <!--         第一种，普通值注入 value-->
           <property name="name" value="喵不易"/>
   
       </bean>
   
   </beans>
   ```

4. 测试类

   ```java
   import com.miaobuyi.pojo.Student;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class MyTest {
       public static void main(String[] args) {
           ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           Student student = (Student) context.getBean("student");
           System.out.println(student);
       }
   }
   ```

完善注入信息

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="address" class="com.miaobuyi.pojo.Address">
        <property name="address" value="成都"/>
    </bean>

    <bean id="student" class="com.miaobuyi.pojo.Student">
<!--         第一种，普通值注入 value-->
        <property name="name" value="喵不易"/>

<!--        第二种，Bean注入 ref-->
        <property name="address" ref="address"/>

        <!-- 数组注入-->
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>西游记</value>
                <value>水浒传</value>
                <value>三国演义</value>
            </array>
        </property>

        <!-- List注入-->
        <property name="hobbys">
            <list>
                <value>听歌</value>
                <value>敲代码</value>
                <value>看电影</value>
            </list>
        </property>

        <!-- Map注入-->
        <property name="card">
            <map>
                <entry key="省份证" value="1321321321"/>
                <entry key="银行卡" value="64654654"/>
            </map>
        </property>

        <!-- Set注入-->
        <property name="games">
            <set>
                <value>LOL</value>
                <value>COC</value>
                <value>BOB</value>
            </set>
        </property>

        <!-- Null注入-->
        <property name="wife">
            <null/>
        </property>

        <!-- Properties注入-->
        <property name="info">
            <props>
                <prop key="学号">15846532</prop>
                <prop key="性别">男</prop>
                <prop key="姓名">张三</prop>
            </props>
        </property>
    </bean>

</beans>
```



### 3、拓展方式注入

我们可以使用p命名空间和c命名空间进行注入

官方解释：

![image-20210720195043436](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210720195043436.png)





```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--p命名空间注入，可以直接注入属性的值：properties-->
    <bean id="user" class="com.miaobuyi.pojo.User" p:name="喵不易" p:age="18"/>

    <!--c命名空间注入，可以直接注入属性的值：construct-->
    <bean id="user2" class="com.miaobuyi.pojo.User" c:age="18" c:name="喵不易"/>

</beans>
```

测试：

```java
@Test
public void test2(){
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
    User user = context.getBean("user2", User.class);
    System.out.println(user);
}
```

注意点：p命名和c命名不能直接使用，需要导入xml约束 



### 4、Bean的作用域

 ![image-20210720205700969](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210720205700969.png)

1. 单例模式（Spring默认机制）

   ```xml
   <bean id="user2" class="com.miaobuyi.pojo.User" c:age="18" c:name="喵不易" scope="singleton"/>
   ```

2. 原型模式：每次从容器中get的时候，都会产生一个新对象！

   ```xml
   <bean id="user2" class="com.miaobuyi.pojo.User" c:age="18" c:name="喵不易" scope="prototype"/>
   ```

3. 其余的 request、session、application这些只能在web开放中使用

 

## 7、Bean的自动装配

* 自动装配是Spring满足Bean依赖的一种方式！
* Spring会在上下文中自动寻找，并自动给bean装配属性！



在Spring中有三种装配的方式

1. 在xml中显示的配置
2. 在Java中显示配置
3. 隐式的自动装配bean【重要】



### 1、测试

环境搭建：一个人有两个宠物！



### 2、byName自动装配

```xml
<bean id="cat" class="com.miaobuyi.pojo.Cat"/>
<bean id="dog" class="com.miaobuyi.pojo.Dog"/>

<!--
byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的bean id
-->
<bean id="people" class="com.miaobuyi.pojo.People" autowire="byName">
    <property name="name" value="喵不易"/>
</bean>
```



### 3、byType自动装配

```xml
<bean id="cat" class="com.miaobuyi.pojo.Cat"/>
<bean id="dog11" class="com.miaobuyi.pojo.Dog"/>

<!--
byType:会自动在容器上下文中查找，和自己对象属性类型相同的bean
-->
<bean id="people" class="com.miaobuyi.pojo.People" autowire="byType">
    <property name="name" value="喵不易"/>
</bean>
```

小结：

* byName的时候，需要保证所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致！
* byType的时候，需要保证所有bean的class唯一，并且这个bean需要和自动注入的属性的类型一致！

### 4、使用注解实现自动装配

autowire 先byType，如果类型大于1，再byName

jdk1.5支持的注解，Spring2.5就支持注解了！

基于注释的配置的引入提出了一个问题，即这种方法是否比XML“更好”。

要使用注解须知：

1. 导入约束 context约束
2. 配置注解的支持：==<context: annotation-config/>==

``` xml
<?xm1 version="1. 0" encoding="UTF-8"?>
<beans xm7ns="http://www.springframework.org/ schema/beans"
xm1ns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xm1ns:context="http://www.springframework.org/schema/context"
xsi:schemalocati on="http: / /www. springf ramework. org/ schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springf ramework.org/schema/context
    https://www.springf ramework.org/schema/context/spring-context. xsd">
    
<!--开启注解的支持-->    
<context: annotation-config/>
    
</beans>|

```



@Autowired

直接在属性上使用即可！也可以在set方式上使用！

使用Autowired我们可以不用编写set方法了，前提是你这个自动装配的属性在IOC容器（Spring）容器中存在。且符合byName和byType

科普：

``` java
@Nullable 字段标记了这个注解，说明这个字段可以为null
```

```java
public @interface Autowired {
    boolean required() default true;
}
```

如果显示定义了Autowired的required属性为false,说明这个对象可以为null，否则不允许为空

``` java
@Autowired(rerequired = false)
```



测试代码

``` java
public class People {
    //如果显示定义了Autowi red的requi red属性为false，说明这个对象可以为nu11，否则不允许为空
    @Autowired(required = false)
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
}

```



如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候、我们可以使用@Qualifier(value="xxx")去配置@Autowired的使用，指定一个唯一的bean对象注入! 

``` java
pub1ic class People {
    @Autowired
    @Qualifier(va1ue="cat111")
    private Cat cat;
    @Autowi red
    @Qualifier(va1ue="dog222")
    private Dog dog;
    private String name;
}
```

**@Resource注解**

``` java
pub1ic class Peop1e {

    @Resource(name = "cat2")
    private Cat cat;
    @Resource
    private Dog dog;

}
```

jdk11没有@Resource

小结：

autowired是先byteType,如果唯一則注入，否则byName查找。

resource是先byname,不符合再继续byType



## 8、使用注解开发



**注解说明**：

- @Autowired :自动装配通过类型。名字如果Autowired不能唯一自动装配 上属性，则需要通过@Qualifier(value=" xxx" )
- @Nu1lable字段标记 了这个注解，说明这个字段可以为nu11; .
- @Resource:自动装配通过名字。类型。



- @Component :组件，放在类上，说明这个类被Spring管理了，就是bean!







在Spring4之后，要使用注解开发，AOP的包必须导入

使用注解需要导入context约束，增加注解的支持！



1. bean

2. 属性如何注入

   ```java
   //等价于    <bean id="user" class="com.miaobuyi.pojo.User"/>
   @Component
   public class User {
       //相当于<property name="name" value="喵不易"/>
       @Value("miaobuyi")
       public String name;
   
       public void setName(String name) {
           this.name = name;
       }
   ```

   

3. 衍生的注解

   @Component有几个衍生注解，我们在web开发中，会按照mvc三层架构分层

   * dao【@Repository】
   * service【@Service】
   * controller【@Controller】

   这四个注解功能都是一样的，都是代表将某个类注册到Spring中，装配Bean

4. 自动装配

   - @Autowired :自动装配通过类型。名字如果Autowired不能唯一自动装配 上属性，则需要通过@Qualifier(value=" xxx" )
   - @Nu1lable字段标记 了这个注解，说明这个字段可以为nu11; .
   - @Resource:自动装配通过名字。类型。

5. 作用域

   ```java
   @Component
   @Scope("prototype")
   public class User {
       //相当于<property name="name" value="喵不易"/>
       @Value("miaobuyi")
       public String name;
   
       public void setName(String name) {
           this.name = name;
       }
   }
   ```

6. 小结

 xml与注解：

* xml更加万能，适用于任何场合！维护简单方便
* 注解，不是自己的类不能使用，维护相对复杂

xml与注解最佳实践：

* xml用来管理bean；
* 注解只负责完成属性的注入；
* 我们在使用的过程中，只需要注意一个问题：必须让注解生效，就需要开启注解支持

```xml
<!--指定要扫描的包，这个包下的注解就会生效-->
<context:component-scan base-package="com.miaobuyi"/>
<context:annotation-config/>
```



## 9、使用java的方式配置Spring

我们现在要完全不使用Spring的xml配置了，全权交给java来做！

javaConfig是Spring的一个子项目，在Spring4之后，它成为了核心功能

![image-20210722194702296](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210722194702296.png)

实体类

```java
package com.miaobuyi.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

//这里这个注解的意思，就是说明这个类被Spring接管了，注册到了容器中
@Component
public class User {

    @Value("miaobuyi")
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}

```

配置文件

``` java
package com.miaobuyi.config;


import com.miaobuyi.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

//这个也会Spring容器托管，注册到容器中，应为他本来就是一个@Component，@Configuration代表这是一个配置类，就和我们之前看的beans.xml一样的
@Configuration
@ComponentScan("com.miaobuyi")
@Import(MyConfig2.class)
public class MyConfig {

    //注册一个bean，就相当于我们之前的一个bean标签
    //这个方法的名字，就相当于bean标签中的id属性
    //这个方法的返回值，就相当于bean标签中的class属性
    @Bean
    public User getUser(){
        return new User();//就是要返回要注入到bean的对象
    }
}

```

测试类

``` java
import com.miaobuyi.config.MyConfig;
import com.miaobuyi.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MyTest {
    //如果完全使用了配置类方式去做，我们就只能通过AnnotationConfig 上下文来获取容器，通过配置类的class对象加载
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
        User user = context.getBean("getUser", User.class);
        System.out.println(user.getName());
    }
}

```



这种纯java的配置方式，在SpringBoot中随处可见！



## 10、代理模式

为什么要学习代理模式？因为这就是SpringAOP的底层！【SpringAOP和SpringMVC】

代理模式的分类：

* 静态代理
* 动态代理

![image-20210722212902919](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210722212902919.png)

### 1、 静态代理

角色分析：

* 抽象角色：一般使用接口或者抽象类来解决
* 真实角色：被代理的角色
* 代理角色：代理真实角色，代理正式角色后，我们一般会做一些附属操作
* 客户：访问代理对象的人



代码步骤：

1. 接口

   ```java
   package com.miaobuyi.demo01;
   
   //租房
   public interface Rent {
       public void rent();
   }
   ```

2. 真实角色

   ```java
   package com.miaobuyi.demo01;
   
   //房东
   public class Host implements Rent{
   
       @Override
       public void rent() {
           System.out.println("房东要出租房子");
       }
   }
   ```

3. 代理角色

   ```java
   package com.miaobuyi.demo01;
   
   public class Proxy implements Rent{
       private Host host;
   
       public Proxy() {
       }
   
       public Proxy(Host host) {
           this.host = host;
       }
   
       @Override
       public void rent() {
           seeHouse();
           host.rent();
           hetong();
           fare();
       }
   
       //看房
       public void seeHouse(){
           System.out.println("中介带你看房");
       }
   
       //签合同
       public void hetong(){
           System.out.println("签合同");
       }
   
       //收中介费
       public void fare(){
           System.out.println("收中介费");
       }
   }
   ```

4. 客户端访问代理角色

   ```java
   package com.miaobuyi.demo01;
   
   public class client {
       public static void main(String[] args) {
           //租客要租房子，需要一个房东
           Host host = new Host();
           //代理，中介帮租客租房子，但是，代理角色一般会有一些附属操作
           Proxy proxy = new Proxy(host);
   
           //你不用面对房东直接找中介租房
           proxy.rent();
       }
   }
   ```



代理模式的好处：

* 可以使真实角色的操作更加纯粹！不用去关注一些公共的业务
* 公共也就交给了代理角色！实现了业务的分工！
* 公共业务防身个拓展的时候，方便集中管理！

缺点：

* 一个真实角色就会产生一个代理角色；代码量会翻~开发效率会贬变低



### 2、加深理解

代码步骤：

1. 接口

   ```java
   package com.miaobuyi.demo02;
   
   public interface UserService {
       public void add();
       public void delete();
       public void update();
       public void query();
   }
   
   ```

2. 真实角色

   ```java
   package com.miaobuyi.demo02;
   
   //真实对象
   public class UserServiceImpl implements UserService{
       @Override
       public void add() {
           System.out.println("增加了一个用户");
       }
   
       @Override
       public void delete() {
           System.out.println("删除了一个用户");
       }
   
       @Override
       public void update() {
           System.out.println("修改了一个用户");
       }
   
       @Override
       public void query() {
           System.out.println("查询了一个用户");
       }
   
       //1.改动原有的业务代码，在公司中是大忌！
   }
   
   ```

3. 代理角色

   ```java
   package com.miaobuyi.demo02;
   
   public class UserServiceProxy implements UserService{
   
       private UserServiceImpl userService;
   
       public void setUserService(UserServiceImpl userService) {
           this.userService = userService;
       }
   
       @Override
       public void add() {
           log("add");
           userService.add();
       }
   
       @Override
       public void delete() {
           log("delete");
           userService.delete();
       }
   
       @Override
       public void update() {
           log("update");
           userService.update();
       }
   
       @Override
       public void query() {
           log("query");
           userService.query();
       }
   
       //加一个日志方法
       public void log(String msg){
           System.out.println("[Debug] 使用了"+msg+"方法");
       }
   }
   
   ```

4. 客户端访问代理角色

   ```java
   package com.miaobuyi.demo02;
   
   public class Client {
       public static void main(String[] args) {
           UserServiceImpl userService = new UserServiceImpl();
           UserServiceProxy proxy = new UserServiceProxy();
           proxy.setUserService(userService);
           proxy.add();
       }
   
   
   }
   
   ```



聊聊AOP

![image-20210722221543080](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210722221543080.png)

### 3、动态代理

* 动态代理和静态代理角色一样
* 动态代理的代理类是动态生成的，不是我们写好的！
* 动态代理可以分为两大类：基于接口的动态代理，基于类的动态代理
  * 基于接口---JDK动态代理【我们在这里使用】
  * 基于类---cglib
  * Java字节码实现：javassist



需要了解两个类：Proxy：代理，InvocationHandler：调用处理程序



动态代理的好处：

* 可以使真实角色的操作更加纯粹！不用去关注一些公共的业务
* 公共也就交给了代理角色！实现了业务的分工！
* 公共业务防身个拓展的时候，方便集中管理！
* 一个动态代理类代理的是一个接口，一般对应的一类业务
* 一个动态代理类可以代理多个类，只要实现了同一个接口即可



## 11、AOP



**横向编程的思想，在不影响原来业务类的情况下实现动态增强**

### 1、什么是AOP

AOP (Aspect Oriented Programming)意为:面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP 是OOP的延续,是软件开发中的一个热点,也是Spring框架中的一-个重要内容,是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

![image-20210726160535693](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210726160535693.png)

### 2、Aop在Spring中的作用

==提供声明式事务;允许用户自定义切面==

* 横切关注点:跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志，安全,缓存,事务等等...
* 切面(ASPECT) :横切关注点被模块化的特殊对象。即，它是一个类。
* 通知(Advice) :切面必须要完成的工作。即，它是类中的-一个方法。
* 目标(Target) :被通知对象。
* 代理(Proxy) :向目标对象应用通知之后创建的对象。
* 切入点(PointCut) :切面通知执行的“地点”的定义。
* 连接点(JointPoint) :与切入点匹配的执行点。

![image-20210726190134497](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210726190134497.png)

SpringAOP中，通过Advice定义横切逻辑，Spring中支持5中类型的Advice：

![image-20210726203150571](C:\Users\杨宇\AppData\Roaming\Typora\typora-user-images\image-20210726203150571.png)

即AOP在不盖面原有代码的情况下，去增加新的功能

### 3、使用Spring实现AOP

【重点】使用AOP织入，需要一个依赖包

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj </groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>

```





#### 方式一：使用Spring的API接口【主要是SpringAPI接口实现】

```java
package com.miaobuyi.log;

import org.springframework.aop.AfterReturningAdvice;

import java.lang.reflect.Method;

public class AfterLog  implements AfterReturningAdvice {

    /**
     *
     * @param returnValue 返回值
     * @param method
     * @param args
     * @param target
     * @throws Throwable
     */
    @Override
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行了"+method.getName()+"方法，返回结果为："+returnValue);
    }
}

```

```java
package com.miaobuyi.log;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

public class log implements MethodBeforeAdvice {

    //method:要执行的目标对象的方法
    //args：参数
    //target：目标对象
    @Override
    public void before(Method method, Object[] objects, Object target) throws Throwable {
        System.out.println(target.getClass().getName()+"的"+method.getName()+"被执行了");
    }
}
```

```java
package com.miaobuyi.service;

public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void select();
}
```

```java
package com.miaobuyi.service;

public class UserServiceImpl implements UserService{
    @Override
    public void add() {
        System.out.println("增加了一个用户");
    }

    @Override
    public void delete() {
        System.out.println("删除了一个用户");
    }

    @Override
    public void update() {
        System.out.println("更新了一个用户");
    }

    @Override
    public void select() {
        System.out.println("查询了一个用户");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">


    <!--注册bean-->
    <bean id="userService" class="com.miaobuyi.service.UserServiceImpl"/>
    <bean id="log" class="com.miaobuyi.log.log"/>
    <bean id="afterLog" class="com.miaobuyi.log.AfterLog"/>

    <!--方式一：使用Spring原生Spring API接口-->
    <!--配置aop:需要导入aop的约束-->
    <aop:config>
        <!--切入点 expression:表达式,execution(要执行的位置！ * * * *)-->
        <aop:pointcut id="pointcut" expression="execution(* com.miaobuyi.service.UserServiceImpl.*(..))"/>

        <!--执行环绕增强-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>

</beans>
```



#### 方式二：自定义类来实现AOP【主要是切面定义】

```java
package com.miaobuyi.diy;

public class DiyPointCut {
    public void before(){
        System.out.println("==========方法执行前==========");
    }

    public void after(){
        System.out.println("==========方法执行后==========");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">


    <!--注册bean-->
    <bean id="userService" class="com.miaobuyi.service.UserServiceImpl"/>
    <bean id="log" class="com.miaobuyi.log.log"/>
    <bean id="afterLog" class="com.miaobuyi.log.AfterLog"/>

    <!--方式一：使用Spring原生Spring API接口-->
    <!--配置aop:需要导入aop的约束-->
<!--    <aop:config>-->
<!--        &lt;!&ndash;切入点 expression:表达式,execution(要执行的位置！ * * * *)&ndash;&gt;-->
<!--        <aop:pointcut id="pointcut" expression="execution(* com.miaobuyi.service.UserServiceImpl.*(..))"/>-->

<!--        &lt;!&ndash;执行环绕增强&ndash;&gt;-->
<!--        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>-->
<!--        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>-->
<!--    </aop:config>-->

    <!--方式二：自定义类-->
    <bean id="diyPointCut" class="com.miaobuyi.diy.DiyPointCut"/>
    <aop:config>
        <!--自定义切面，ref 要引用的类-->
        <aop:aspect ref="diyPointCut">
            <!--切入点-->
            <aop:pointcut id="point" expression="execution(* com.miaobuyi.service.UserServiceImpl.*(..))"/>
            <!--通知-->
            <aop:before method="before" pointcut-ref="point"/>
            <aop:after method="after" pointcut-ref="point"/>
        </aop:aspect>
    </aop:config>
</beans>
```

#### 方式三：使用注解

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">


    <!--注册bean-->
    <bean id="userService" class="com.miaobuyi.service.UserServiceImpl"/>
    <bean id="log" class="com.miaobuyi.log.log"/>
    <bean id="afterLog" class="com.miaobuyi.log.AfterLog"/>

    <!--方式一：使用Spring原生Spring API接口-->
    <!--配置aop:需要导入aop的约束-->
<!--    <aop:config>-->
<!--        &lt;!&ndash;切入点 expression:表达式,execution(要执行的位置！ * * * *)&ndash;&gt;-->
<!--        <aop:pointcut id="pointcut" expression="execution(* com.miaobuyi.service.UserServiceImpl.*(..))"/>-->

<!--        &lt;!&ndash;执行环绕增强&ndash;&gt;-->
<!--        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>-->
<!--        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>-->
<!--    </aop:config>-->

    <!--方式二：自定义类-->
<!--    <bean id="diyPointCut" class="com.miaobuyi.diy.DiyPointCut"/>-->
<!--    <aop:config>-->
<!--        &lt;!&ndash;自定义切面，ref 要引用的类&ndash;&gt;-->
<!--        <aop:aspect ref="diyPointCut">-->
<!--            &lt;!&ndash;切入点&ndash;&gt;-->
<!--            <aop:pointcut id="point" expression="execution(* com.miaobuyi.service.UserServiceImpl.*(..))"/>-->
<!--            &lt;!&ndash;通知&ndash;&gt;-->
<!--            <aop:before method="before" pointcut-ref="point"/>-->
<!--            <aop:after method="after" pointcut-ref="point"/>-->
<!--        </aop:aspect>-->
<!--    </aop:config>-->

    <!--方式三-->
    <bean id="annotationPointCut" class="com.miaobuyi.diy.AnnotationPointCut"/>
    <!--开启注解支持  JDK(默认 proxy-target-class="false")  cglib（proxy-target-class="true"）-->
    <aop:aspectj-autoproxy/>
</beans>
```

```java
package com.miaobuyi.diy;

//方式三 使用注解方式实现AOP

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect//标注这个类是一个切面
public class AnnotationPointCut {
    @Before("execution(* com.miaobuyi.service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("==========方法执行前==========");
    }
    @After("execution(* com.miaobuyi.service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("==========方法执行后==========");
    }

    //在环绕增强中，我们可以给定一个参数，代表我们要获取处理切入的点
    @Around("execution(* com.miaobuyi.service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("环绕前");

        Signature signature = joinPoint.getSignature();//获得签名
        System.out.println(signature);
        //执行方法
        Object proceed = joinPoint.proceed();

        System.out.println("环绕后");

        System.out.println(proceed);
    }
}
```



## 12、整合Mybatis

步骤：

1. 导入相关jar包

   * junit
   * mybatis
   * mysql数据库
   * Spring相关的
   * aop织入
   * mybatis-Spring【new】

   ```xml
   <dependencies>
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.13</version>
       </dependency>
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>8.0.25</version>
       </dependency>
       <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis</artifactId>
           <version>3.5.7</version>
       </dependency>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-webmvc</artifactId>
           <version>5.3.9</version>
       </dependency>
       <!--Spring操作数据库的话还需要一个spring-jdbc-->
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-jdbc</artifactId>
           <version>5.3.8</version>
       </dependency>
       <dependency>
           <groupId>org.aspectj</groupId>
           <artifactId>aspectjweaver</artifactId>
           <version>1.9.7</version>
       </dependency>
       <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis-spring</artifactId>
           <version>2.0.6</version>
       </dependency>
   </dependencies>
   ```

2. 编写配置文件

3. 测试



### 1、回忆Mybatis

1. 编写实体类
2. 编写核心配置文件
3. 编写接口
4. 编写Mapper.xml
5. 测试



### 2、Mybatis-spring



1. 编写数据源
2. sqlSessionFactory
3. sqlSessionTemplate
4. 需要给接口加实现类【】
5. 将自己写的实现类，注入到Spring中
6. 测试使用即可

spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--DataSource：使用Spring的数据源替换Mybatis的配置 c3p0 dbcp druid
    我们这里使用Spring提供的JDBC
    -->

    <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </bean>

    <!--SqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource"/>
        <!--绑定mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/miaobuyi/mapper/UserMapper.xml"/>
    </bean>

    <!--SqlSessionTemplate:就是我们使用的sqlSession-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!--只能使用构造器注入sqlSessionFactory，因为他没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>

    <!---->
    <bean id="UserMapper" class="com.miaobuyi.mapper.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"></property>
    </bean>
</beans>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

    <import resource="spring-dao.xml"/>

    <!---->
    <bean id="UserMapper" class="com.miaobuyi.mapper.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"></property>
    </bean>
</beans>
```

UserMapperImpl.java

```java
package com.miaobuyi.mapper;

import com.miaobuyi.pojo.User;
import org.mybatis.spring.SqlSessionTemplate;

import java.util.List;

public class UserMapperImpl implements UserMapper{

    //我们所有操作，在原来都使用sqlSession来执行，现在都使用SqlSessionTemplate
    private SqlSessionTemplate sqlSession;

    public void setSqlSession(SqlSessionTemplate sqlSession) {
        this.sqlSession = sqlSession;
    }

    @Override
    public List<User> selectUser() {
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.selectUser();
    }
}
```

UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.miaobuyi.mapper.UserMapper">

    <select id="selectUser" resultType="user">
        select * from mybatis.user
    </select>

</mapper>
```

UserMapper.java

```java
package com.miaobuyi.mapper;

import com.miaobuyi.pojo.User;

import java.util.List;

public interface UserMapper {
    public List<User> selectUser();
}
```

测试

```java
import com.miaobuyi.mapper.UserMapper;
import com.miaobuyi.pojo.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class MyTest {

    @Test
    public void test() throws IOException {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring-dao.xml");
        UserMapper userMapper = context.getBean("UserMapper", UserMapper.class);
        for (User user : userMapper.selectUser()) {
            System.out.println(user);
        }
    }
}
```



方式二

通过继承获取SqlSession

```java
package com.miaobuyi.mapper;

import com.miaobuyi.pojo.User;
import org.mybatis.spring.support.SqlSessionDaoSupport;

import java.util.List;

public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper{

    @Override
    public List<User> selectUser() {

        return getSqlSession().getMapper(UserMapper.class).selectUser();
    }
}
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

    <import resource="spring-dao.xml"/>

    <!---->
    <bean id="UserMapper" class="com.miaobuyi.mapper.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"></property>
    </bean>

    <bean id="UserMapper2" class="com.miaobuyi.mapper.UserMapperImpl2">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>
</beans>
```



## 13、声明式事务

### 1、回顾事务

* 把一组业务当成一个业务来做：要么都成功，要么都失败！
* 事务在项目开发中，十分的重要，涉及到数据的一致性问题，不能马虎！
* 确保完整性和一致性



事务ACID原则：

* 原子性
* 一致性
* 隔离性
  * 多个业务可能操作同一个资源，防止数据损坏
* 持久性
  * 事务一旦体检，无论系统发生什么问题，结果都不会再被影响，被持久化的写到存储器中！



2、Spring当中的事务管理

* 声明式事务：AOP
* 编程式事务：需要在代码中，进行事务的管理

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--DataSource：使用Spring的数据源替换Mybatis的配置 c3p0 dbcp druid
    我们这里使用Spring提供的JDBC
    -->

    <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </bean>

    <!--SqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource"/>
        <!--绑定mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/miaobuyi/mapper/UserMapper.xml"/>
    </bean>

    <!--SqlSessionTemplate:就是我们使用的sqlSession-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!--只能使用构造器注入sqlSessionFactory，因为他没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>

    <!--配置声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="datasource"/>
    </bean>

    <!--结合AOP实现事务的织入-->
    <!--配置事务通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--给那些方法配置事务-->
        <!--配置事务的传播特性：new  propagation= -->
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

    <!--配置事务切入-->
    <aop:config>
        <aop:pointcut id="txPoint" expression="execution(* com.miaobuyi.mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPoint"/>
    </aop:config>

</beans>
```



思考：

为什么需要事务？

* 如果不配置事务，可能存在数据提交不一致的情况下；
* 如果我们不在Spring中去配置声明式事务，我们就需要在代码中手动配置事务！
* 事务在项目的开发中十分重要，涉及到数据的一致性和完整性问题，不容马虎

