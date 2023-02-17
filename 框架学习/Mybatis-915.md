# Mybatis-915

# 1、入门

### 1、什么是Mybatis

MyBatis 是一款优秀的持久层框架

MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集的过程

MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 实体类 【Plain Old Java Objects,普通的 Java对象】映射成数据库中的记录。

MyBatis 本是apache的一个开源项目ibatis, 2010年这个项目由apache 迁移到了google code，并且改名为MyBatis 。

2013年11月迁移到Github 。

Mybatis官方文档 : http://www.mybatis.org/mybatis-3/zh/index.html

GitHub : https://github.com/mybatis/mybatis-3

### 2、什么是持久层？

完成持久化工作的代码块 . ----> dao层 【DAO (Data Access Object) 数据访问对象】

大多数情况下特别是企业级应用，数据持久化往往也就意味着将内存中的数据保存到磁盘上加以固化，而持久化的实现过程则大多通过各种关系数据库来完成。

不过这里有一个字需要特别强调，也就是所谓的“层”。对于应用系统而言，数据持久功能大多是必不可少的组成部分。也就是说，我们的系统中，已经天然的具备了“持久层”概念？也许是，但也许实际情况并非如此。之所以要独立出一个“持久层”的概念,而不是“持久模块”，“持久单元”，也就意味着，我们的系统架构中，应该有一个相对独立的逻辑层面，专注于数据持久化逻辑的实现.

与系统其他部分相对而言，这个层面应该具有一个较为清晰和严格的逻辑边界。【说白了就是用来操作数据库存在的！】

### 3、为什么需要Mybatis?

Mybatis就是帮助程序猿将数据存入数据库中 , 和从数据库中取数据 .

传统的jdbc操作 , 有很多重复代码块 .比如 : 数据取出时的封装 , 数据库的建立连接等等... , 通过框架可以减少重复代码,提高开发效率 .

MyBatis 是一个半自动化的ORM框架 (Object Relationship Mapping) -->对象关系映射

所有的事情，不用Mybatis依旧可以做到，只是用了它，所有实现会更加简单！技术没有高低之分，只有使用这个技术的人有高低之别

MyBatis的优点

简单易学：本身就很小且简单。没有任何第三方依赖，最简单安装只要两个jar文件+配置几个sql映射文件就可以了，易于学习，易于使用，通过文档和源代码，可以比较完全的掌握它的设计思路和实现。

灵活：mybatis不会对应用程序或者数据库的现有设计强加任何影响。sql写在xml里，便于统一管理和优化。通过sql语句可以满足操作数据库的所有需求。

解除sql与程序代码的耦合：通过提供DAO层，将业务逻辑和数据访问逻辑分离，使系统的设计更清晰，更易维护，更易单元测试。sql和代码的分离，提高了可维护性。

提供xml标签，支持编写动态sql。

.......

## 2、Mybatis第一个程序

思路流程：环境搭建--->导入Mybatis--->编写代码--->测试

### 1、搭建数据库

```sql
CREATE DATABASE `mybatis`;

use mybatis;

CREATE TABLE IF EXISTS `user`;

CREATE TABLE `user`(
	`id` int(20) NOT NULL PRIMARY KEY,
    `name` varchar(255) DEFUALT NULL,
    `pwd` varchar(255) DEFUALT NULL
)ENGING=INNODB DEFUALT CHARSET = utf8;
```

### 2、导入Mybatis相关的jar包

```xml
<dependencies>
       <!--mysql依赖-->
       <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.25</version>
       </dependency>
       <!--mybatis依赖-->
       <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <!--junit依赖-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>   
```



## 3、CRUD

### 1、namespace

namespace中的包名要和Dao/mapper接口的包名一致！

### 2、select

选择，查询语句

id：就是对应namespace中的方法名；

resultType:Sql语句的返回值！

parameterType：参数类型！

1. 编写接口

   ```java
   // 根据ID查询用户
   User getUserById(int id);
   ```

   

2. 编写对应的mapper中的sql语句

   ```xml
   <select id="getUserById" parameterType="int" resultType="com.chen.pojo.User" >
       select * from mybatis.user where id = #{id}
   </select>
   ```

3. 测试

   ```java
   @Test
   public void getUserById() {
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           User user = mapper.getUserById(3);
           System.out.println(user);
           sqlSession.close();
       }
   ```

   

### 3、Insert

```xml
<insert id="addUser" parameterType="com.chen.pojo.User">
    insert into mybatis.user (id, name, pwd) VALUES (#{id},#{name},#{pwd})
</insert>
```

### 4、Update

```xml
<update id="updateUser" parameterType="com.chen.pojo.User">
    update mybatis.user
        set name= #{name},pwd = #{pwd}
    where id = #{id};
</update>
```

### 5、Delete

```xml
<delete id="deleteUser" parameterType="int">
    delete from mybatis.user where id = #{id}
</delete>
```

注意：增删改需要提交事务

### 6、万能的Map

假设，我们的实体类，数据库中的表中的字段参数过多，我们应当考虑使用Map！

```java
// 万能Map
int addUser2(Map<String,Object> map);
```

```xml
<!--对象中的属性可以直接取出来-->
<insert id="addUser2" parameterType="map">
    insert into mybatis.user (id, name, pwd) VALUES (#{userid},#{username},#{userpwd})
</insert>
```

```java
@Test
public void addUser2() {
     SqlSession sqlSession = MybatisUtils.getSqlSession();
     UserMapper mapper = sqlSession.getMapper(UserMapper.class);
     Map<String,Object> map = new HashMap<String, Object>();
     map.put("userid",7);
     map.put("username","宋岳衡");
     map.put("userpwd","12345");
     mapper.addUser2(map);
     sqlSession.commit();
     sqlSession.close();
    }
```

map传递参数，直接在sql中取出key即可！

对象传递参数，直接在sql中去对象属性即可!

只有一个基本类型参数情况下，可以直接在sql中取到！

多个参数用map**或者注解**

### 7、模糊查询

1. 在java代码执行的时候使用通配符% %

   ```java
   List<User> userList = mapper.getUserLike("%李%");
   ```

2. 在SQL拼接中使用通配符

   ```xml
   <select id="getUserLike" resultType="com.chen.pojo.User">
           select * from mybatis.user where name like "%"#{value}"%"
       </select>
   ```

   

## 4、配置解析

### 1、核心配置文件

1. mybatis-config.xml
2. Mybatis的配置文件包含了会深深影响Mybatis行为的设置和属性信息。

```java
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

MyBatis 可以配置成适应多种环境，这种机制有助于将 SQL 映射应用于多种数据库之中， 现实情况下有多种理由需要这么做。

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

Mybatis默认事务管理器就是JDBC，连接池：POOLED

### 3、属性（properties）

我们可以通过properties属性来实现引用配置文件

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。【db.properties】

编写一个配置文件

db.properties

```properties
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?user=true&useUnicode=true&characterEncoding=UTF-8
username=root
password=zjch1328
```

在核心配置文件中引入

```xml
<!--引入外部配置文件-->
<properties resource="db.properties"/>
```

可以直接引入外部文件

可以在其中增加一些属性配置

如果两个文件有同一个字段，优先使用外部配置文件的！

### 4、类型别名（typeAliases）

类型别名可为 Java 类型设置一个缩写名字。 

它仅用于 XML 配置，意在降低冗余的全限定类名书写。

```xml
<!--可以给实体类起别名-->
<typeAliases>
    <typeAlias type="com.chen.pojo.User" alias="User"/>
</typeAliases>
```

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如：

扫描实体类的包，它默认别名就是这个类的类名，首字母小写！

```xml
<!--可以给实体类起别名-->
<typeAliases>
    <package name="com.chen.pojo"/>
</typeAliases>
```

在实体类比较少的情况下，使用第一种方式。

如果实体类比较多，建议使用第二种。

区别：第一种可以DIY，第二种需要在相应的实体类上加注解

```java
// 实体类
@Alias("user")
public class User {
    private int id;
    private String name;
    private String pwd;

```

### 5、设置

这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。

| cacheEnabled       | 全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。     | true \| false | true  |
| ------------------ | ------------------------------------------------------------ | ------------- | :---: |
| lazyLoadingEnabled | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 `fetchType` 属性来覆盖该项的开关状态。 | true \| false | false |

| defaultResultSetType | 指定语句默认的滚动策略。（新增于 3.5.2） | FORWARD_ONLY \| SCROLL_SENSITIVE \| SCROLL_INSENSITIVE \| DEFAULT（等同于未设置） | 未设置 (null) |
| -------------------- | ---------------------------------------- | ------------------------------------------------------------ | ------------- |

### 6、映射器（mappers）

MapperRegistry:注册绑定我们的Mapper文件。

方式一：

```xml
<!--每一个Mapper.XML都需要在Mybatis核心配置文件中注册!-->
<mappers>
    <mapper resource="com/chen/dao/UserMapper.xml"/>
</mappers>
```

方式二：使用class文件绑定注册Mapper文件

```xml
<!--每一个Mapper.XML都需要在Mybatis核心配置文件中注册!-->
<mappers>
    <mapper class="com.chen.dao.UserMapper"/>
</mappers>
```

注意点：

接口和他的Mapper配置文件必须同名

接口和他的Mapper配置文件必须在同一个包下！

方式三：使用扫描包进行注册绑定

```xml
<!--每一个Mapper.XML都需要在Mybatis核心配置文件中注册!-->
<mappers>
    <package name="com.chen.dao"/>
</mappers>
```

注意点：

接口和他的Mapper配置文件必须同名

接口和他的Mapper配置文件必须在同一个包下

### 7、作用域（Scope）和生命周期

生命周期和作用域是至关重要的，因为错误的使用会导致非常严重的**并发问题**。

#### SqlSessionFactoryBuilder

一旦创建了 SqlSessionFactory，就不再需要它了。把它设置成局部变量

 **SqlSessionFactory：**

说白了就是可以想象为：数据库来连接池

SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，**没有任何理由丢弃它或重新创建另一个实例**。

因此 SqlSessionFactory 的最佳作用域是应用作用域

最简单的就是使用单例模式或者静态单例模式。

#### SqlSession

链接到链接池的一个请求！

SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。

用完之后赶紧关闭，否则资源被占用！

## 5、解决属性名和字段名不一致的问题

### 1、问题

新建一个项目，拷贝之前的，测试实体类字段不一致情况

```java
// 实体类
public class User {
    private int id;
    private String name;
    private String password;
}
```

测试出现问题：User{id=3, name='王五', password='null'}

```xml
// select * from mybatis.user where id = #{id}
// 类型处理器
// select id,name,pwd as password from mybatis.user where id = #{id}
```

解决方法：

1. **起别名**

   ```xml
   <select id="getUserById" parameterType="int" resultType="user" >
       select id,name,pwd as password from mybatis.user where id = #{id}
   </select>
   ```

### 2、resultMap(结果集映射)

```xml
id	name	pwd
id	name	password
```

```xml
<!--结果集映射-->
<resultMap id="UserMap" type="User">
    <!--column数据库中的字段，property实体类中的属性-->
    <result column="id" property="id"/>
    <result column="name" property="name"/>
    <result column="pwd" property="password"/>
</resultMap>
<select id="getUserById" resultMap="UserMap" >
    select * from mybatis.user where id = #{id}
</select>
```

1. `resultMap` 元素是 MyBatis 中最重要最强大的元素。
2. ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。

## 6、日志

### 6、1日志工厂

如果一个数据库操作出现了异常，我们需要排错。日志就是最好的帮手。

现在日志工厂！

| defaultResultSetType | 指定语句默认的滚动策略。（新增于 3.5.2） | FORWARD_ONLY \| SCROLL_SENSITIVE \| SCROLL_INSENSITIVE \| DEFAULT（等同于未设置） | 未设置 (null) |
| -------------------- | ---------------------------------------- | ------------------------------------------------------------ | ------------- |

- SLF4J
- Apache Commons Logging
- Log4j 2
- Log4j
- JDK logging
- STDOUT_LOGGING

在Mybatis中具体使用那一个日志实现，在设置中设定！

**STDOUT_LOGGIN标准日志输出**

在mybatis核心配置文件中配置我们的日志！

```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```



### 6、2 LOG4J

什么是log4j:Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件

我们也可以控制每一条日志的输出格式；

通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。

1. 先导入Log4j的包

   ```xml
   <!--导入LOG4J依赖-->
   <dependencies>
       <!-- https://mvnrepository.com/artifact/log4j/log4j -->
       <dependency>
           <groupId>log4j</groupId>
           <artifactId>log4j</artifactId>
            <version>1.2.17</version>
       </dependency>
   </dependencies>
   ```

2. log4j.properties

   ```properties
   #将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
   log4j.rootLogger=DEBUG,console,file
   
   #控制台输出的相关设置
   log4j.appender.console=org.apache.log4j.ConsoleAppender
   log4j.appender.console.Target=System.out
   log4j.appender.console.Threshold=DEBUG
   log4j.appender.console.layout=org.apache.log4j.PatternLayout
   log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
   
   #文件输出的相关设置
   log4j.appender.file=org.apache.log4j.RollingFileAppender
   log4j.appender.file.File=./log/chen.log
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

3. 配置log4j为日志实现

   ```xml
   <settings>
           <setting name="logImpl" value="LOG4J"/>
   </settings>
   ```

4. log4j的使用！**简单使用**

   1. 在要使用Log4j的类中，导入包：import org.apache.log4j.Logger;

   2. 日志对象，参数为当前类的class

      ```java
      static Logger logger = Logger.getLogger(UserDaoTest.class);
      ```

   3. 日志级别

      ```java
      logger.info("info:进入了testLog4j");
      logger.debug("debug:进入了testLog4j");
      logger.error("error:进入了testLog4j");
      ```

## 7、分页

**为什么分页？**

减少数据的处理量

```sql
SELECT * from user limit startIndex,pageSize;
```

使用mybatis实现分页，核心SQL

### 7.1、使用limit分页

1. 接口

   ```java
   // 分页
   List<User> getUserByLimit(Map<String,Integer> map);
   ```

2. Mapper.xml

   ```xml
   <select id="getUserByLimit" resultMap="UserMap" resultType="map">
       select * from mybatis.user limit #{startIndex},#{pageSize}
   </select>
   ```

3. 测试

   ```java
   @Test
   public void getUserByLimit() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       HashMap<String,Integer> map = new HashMap<>();
       map.put("startIndex",0);
       map.put("pageSize",2);
       List<User> userList = mapper.getUserByLimit(map);
       for (User user : userList) {
           logger.info(user);
       }
       sqlSession.close();
   }
   ```

   

### 7.2、RowBounds分页

不再使用SQL实现分页

1. 接口

   ```java
   // 使用RowBounds分页
   List<User> getUserByRowBounds();
   ```

2. mapper.xml

   ```xml
   <select id="getUserByRowBounds" resultMap="UserMap" >
       select * from mybatis.user
   </select>
   ```

3. 测试

   ```java
   @Test
   public void getUserByRowBounds() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
   
       // 通过RowBounds实现
       RowBounds rowBounds = new RowBounds(0,3);
       // 通过java代码层面实现分页
       List<User> userList = sqlSession.selectList("com.chen.dao.UserMapper.getUserByRowBounds",null,rowBounds);
       for (User user : userList) {
           logger.debug(user);
       }
       sqlSession.close();
   }
   ```

## 8、使用注解开发

### 8.1、面向接口编程

大家之前都学过面向对象编程，也学习过接口，但在真正的开发中，很多时候我们会选择面向接口编程

**根本原因 :  解耦 , 可拓展 , 提高复用 , 分层开发中 , 上层不用管具体的实现 , 大家都遵守共同的标准 , 使得开发变得容易 , 规范性更好**

在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己的,对系统设计人员来讲就不那么重要了；

而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。

**关于接口的理解**

接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。

接口的本身反映了系统设计人员对系统的抽象理解。

接口应有两类：

第一类是对一个个体的抽象，它可对应为一个抽象体(abstract class)；

第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）；

一个体有可能有多个抽象面。抽象体与抽象面是有区别的。

**三个面向区别**

面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性及方法 .

面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现 .

接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题.更多的体现就是对系统整体的架构

### 8.2、使用注解开发

1. 注解在接口上实现

   ```java
   @Select("select * from mybatis.user")
   List<User> getUsers();
   ```

2. 需要核心配置文件中绑定接口！

   ```xml
   <!--绑定接口-->
   <mappers>
       <mapper class="com.chen.dao.UserMapper"/>
   </mappers>
   ```

3. 测试

   ```java
   @Test
   public void test() {
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       List<User> users = mapper.getUsers();
       for (User user : users) {
           System.out.println(user);
       }
       sqlSession.close();
   }
   ```

### 8.3、CRUD

我们可以在创建工具类的时候实现自动提交事务！

```java
public static SqlSession getSqlSession() {
    return sqlSessionFactory.openSession(true);
}
```

编写接口，增加注解

```java
// 方法存在多个参数，所有的参数前面必须加上@Param("id")注解
@Select("select * from mybatis.user where id = #{id}")
User getUserByID(@Param("id") int id);

// 增加用户
@Insert("insert into user(id,name,pwd) values (#{id},#{name},#{pwd})")
int addUser(User user);

// 删除用户
@Delete("delete from user where id = #{id}")
int deleteUser(@Param("id") int id);

// 更改用户
@Update("update user set name = #{name}, pwd = #{pwd} where id = #{id}")
int updateUser(User user);
```

测试类

【注意：我们必须要将接口注册绑定到我们的核心配置文件中】



**关于@Param() 注解**

基本类型的参数或者String类型，需要加上

引用数据类型不需要加

如果只有一个基本类型的话，建议加上。

## 9、Lombok

使用步骤

1. 在IDEA中安装Lombok插件！

2. 在项目中导入Lombok的jar包

   ```xml
   <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.12</version>
       <scope>provided</scope>
   </dependency>
   ```

3. 在实体类上加注解！

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
Lombok config system
```

说明：

```java
@Getter and @Setter
@Data:无参构造，set，get，toString,hashCode,equals
@EqualsAndHashCode
@ToString
@RequiredArgsConstructor
@AllArgsConstructor
@NoArgsConstructor
```

## 10、多对一处理

多对一：

多个学生，对应一个老师

对于学生而言，**关联**，多个学生关联一个老师

对于老师而言，**集合**，一个老师，很多个学生

SQL：

```sql
CREATE TABLE `teacher` (
	`id` INT(10) NOT NULL,
	`name` VARCHAR(30) DEFAULT NULL,
	PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO teacher(`id`,`name`) VALUES (1, '陈老师');

CREATE TABLE`student` (
	`id` INT(10) NOT NULL,
	`name` VARCHAR(30) DEFAULT NULL,
	`tid` INT(10) DEFAULT NULL,
	PRIMARY KEY(`id`),
	KEY `fktid` (`tid`),
	CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO student(`id`,`name`,`tid`) VALUES ('1','小张','1');
INSERT INTO student(`id`,`name`,`tid`) VALUES ('2','小明','1');
INSERT INTO student(`id`,`name`,`tid`) VALUES ('3','小赵','1');
INSERT INTO student(`id`,`name`,`tid`) VALUES ('4','小李','1');
INSERT INTO student(`id`,`name`,`tid`) VALUES ('5','小红','1');
INSERT INTO student(`id`,`name`,`tid`) VALUES ('6','小陈','1');
INSERT INTO student(`id`,`name`,`tid`) VALUES ('7','小王','1');
```

### 测试环境搭建

1. 导入Lombok配置！
2. 建立相关实体类 Teacher Student
3. 编写Mapper接口
4. 建立Mapper.XML文件
5. 在核心配置文件里面注册绑定我们的Mapper接口或者文件！
6. 测试查询是否成功！

### 按照查询嵌套处理

```xml
<!--
    思路：
        1、查询所有的学生信息。
        2、根据查询出来的学生tid，寻找对应老师的信息
-->
<select id="getStudent" resultMap="StudentTeacher">
    select * from mybatis.student
</select>
<resultMap id="StudentTeacher" type="Student">
    <result property="id" column="id"/>
    <result property="name" column="name"/>
    <!--复杂的属性，我们需要单独处理
        对象：association
        集合：collection
    -->
    <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
</resultMap>


<select id="getTeacher" resultType="Teacher">
    select * from mybatis.teacher where id = #{id}
</select>
```

### 按照结果嵌套处理

```xml
<!--按照结果嵌套处理-->
<select id="getStudent2" resultMap="StudentTeacher2">
    select s.id sid,s.name sname,t.name tname
    from mybatis.student s, mybatis.teacher t
    where s.tid = t.id
</select>

<resultMap id="StudentTeacher2" type="Student">
    <result property="id" column="sid"/>
    <result property="name" column="sname"/>
    <association property="teacher" javaType="Teacher">
        <result property="name" column="tname"/>
    </association>
</resultMap>
```

**多对一查询方式：联表查询，子查询。**

## 11、一对多处理

比如一个老师拥有多个学生，对老师而言就是一对多的关系。

### **环境搭建和上面一样**

实体类

```java
@Data
public class Student {
    private int id;
    private String name;
    private int tid;

}
```

```java
@Data
public class Teacher {
    private int id;
    private String name;

    // 一个老师拥有多个学生
    private List<Student> students;
}
```

### **按照结果嵌套查询**

```xml
<select id="getTeacher1" resultMap="TeacherStudent">
    select s.id sid,s.name sname,t.name tname,t.id tid
    from mybatis.teacher t,mybatis.student s
    where s.tid = t.id and t.id = #{tid}
</select>

<resultMap id="TeacherStudent" type="Teacher" >
    <result property="id" column="tid"/>
    <result property="name" column="tname"/>
    <collection property="students" ofType="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <result property="tid" column="tid"/>
    </collection>
</resultMap>
```

### 按照查询嵌套处理

```xml
<select id="getTeacher2" resultMap="TeacherStudent2">
    select id,name from mybatis.teacher where id = #{id}
</select>

<resultMap id="TeacherStudent2" type="Teacher">
    <collection property="students" column="id" javaType="ArrayList" ofType="Student" select="getStudentByTeacherId"/>
</resultMap>

<select id="getStudentByTeacherId" resultType="Student">
    select id, name, tid from mybatis.student where tid = #{tid}
</select>
```



### 小结

1. 关联 - association  【多对一】
2. 集合 - collection     【一对多】
3. javaType    &  ofType
   1. JavaType 用来指定实体类中属性的类型
   2. ofType 用来指定映射到List或者集合中的pojo类型，泛型中的约束类型！



注意点：

保证SQL的可读性，尽量通俗易懂。

注意一对多和多对一，属性名和字段名的问题！

如果问题不好排查错误，可以使用日志，建议使用Log4j

### 面试高频

1. MySql引擎
2. InnoDB底层原理
3. 索引
4. 索引优化！

## 12、动态SQL

==**什么是动态SQL：动态SQL就是指根据不同的条件生成不同的SQL语句**==

利用动态SQL这一特性可以彻底摆脱这种痛苦。

```java
如果你之前用过 JSTL 或任何基于类 XML 语言的文本处理器，你对动态 SQL 元素可能会感觉似曾相识。在 MyBatis 之前的版本中，需要花时间了解大量的元素。借助功能强大的基于 OGNL 的表达式，MyBatis 3 替换了之前的大部分元素，大大精简了元素种类，现在要学习的元素种类比原来的一半还要少。
if
choose (when, otherwise)
trim (where, set)
foreach
```



### 搭建环境

```sql
CREATE TABLE `blog` (
	`id` VARCHAR(255) NOT NULL COMMENT `博客id`,
	`title` VARCHAR(255) NOT NULL COMMENT `博客标题`,
	`author` VARCHAR(255) NOT NULL COMMENT `博客作者`,
	`craete_time` datetime NOT NULL COMMENT `创建时间`,
	`views` int(30) NOT NULL COMMENT `浏览量`
) ENGINE=INNODB DEFAULT CHARSET=utf8;
```

### 创建一个基础工程

1. 导包

2. 编写配置文件

3. 编写实体类

   ```java
   @Data
   public class Blog {
       private int id;
       private String title;
       private String author;
       private Date craeteTime;
       private int views;
   }
   ```

4. 编写实体类对应的Mapper接口和Mapper.XML文件

### IF

```xml
<select id="queryBlogIF" parameterType="map" resultType="blog">
    select * from mybatis.blog where 1=1
    <if test="title != null">
        and title = #{title}
    </if>
    <if test="author != null">
        and author = #{author}
    </if>
</select>
```

```java
@Test
public void queryBlogIF() {
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
    HashMap map = new HashMap();
    //map.put("title","java如此简单");
    map.put("author","陈豪");

    List<Blog> blogs = mapper.queryBlogIF(map);
    for (Blog blog : blogs) {
        System.out.println(blog);
    }
    sqlSession.close();
}
```

### choose (when, otherwise)







### **trim (where, set)**

**where**

```xml
<select id="queryBlogIF" parameterType="map" resultType="blog">
    select * from mybatis.blog
    <where>
        <if test="title != null">
            title = #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if>
    </where>
</select>
```

**set**

```xml
<update id="updateBlog" parameterType="map">
    update mybatis.blog
    <set>
        <if test="title != null">
            title = #{title},
        </if>
        <if test="author != null">
            author = #{author},
        </if>
    </set>
    <where>
        id = #{id}
    </where>
</update>
```

**所谓的动态SQL，本质还是SQL语句，只是我们可以在SQL层面，去执行一个逻辑代码**





### sql片段

有时候，我们可能会将一些公共的部分抽取出来，方便复用。

1. 使用SQL标签抽取公共部分

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
   <select id="queryBlogIF" parameterType="map" resultType="blog">
   	select * from mybatis.blog
       	<where>
           	<include refid="if-title-author"></include>
           </where>
   </select>
   ```

注意事项：**最好基于单表来定义SQL片段！** **不要存在where标签**

### **foreach**

```sql
select * from user where 1=1 and 

<foreach item="id" collection="ids"
      open="(" separator="or" close=")">
        #{id}
</foreach>

(id=1 or id=2 or id=3)
```

```xml
<!--
    select * from user where 1=1 and (id=1 or id=2 or id=3)
    传递一个万能的Map，这map中存在一个map集合！
-->
<select id="queryBlogForeach" parameterType="map" resultType="blog">
    select * from mybatis.blog
    <where>
        <foreach collection="ids" item="id" open="and (" separator="or" close=")">
            id = #{id}
        </foreach>
    </where>
</select>
```

==动态SQL就是在拼接SQL语句，我们只要保证SQL的正确性，按照SQL的格式,去排列组合就可以了！==

建议：先在MySql中写出完整的SQL，在对应的去修改成为我们的动态SQL实现通用即可！

## 13、缓存

### 13.1、简介

1. 什么是缓存 [ Cache ]？

   存在内存中的临时数据。

   将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题。

2. 为什么使用缓存？

   减少和数据库的交互次数，减少系统开销，提高系统效率。

3. 什么样的数据能使用缓存？

   经常查询并且不经常改变的数据。

### 13.2、Mybatis缓存

MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。

MyBatis系统中默认定义了两级缓存：一级缓存和二级缓存

默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）

二级缓存需要手动开启和配置，他是基于namespace级别的缓存。

为了提高扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存



### 13.3、一级缓存

一级缓存也叫本地缓存：

与数据库同一次会话期间查询到的数据会放在本地缓存中。

以后如果需要获取相同的数据，直接从缓存中拿，没必须再去查询数据库；

> 测试

1、在mybatis中加入日志，方便测试结果

2、编写接口方法

```java
//根据id查询用户
User queryUserById(@Param("id") int id);
```

3、接口对应的Mapper文件

```sql
<select id="queryUserById" resultType="user">
  select * from user where id = #{id}
</select>
```

4、测试

```java
@Test
public void testQueryUserById(){
   SqlSession session = MybatisUtils.getSession();
   UserMapper mapper = session.getMapper(UserMapper.class);

   User user = mapper.queryUserById(1);
   System.out.println(user);
   User user2 = mapper.queryUserById(1);
   System.out.println(user2);
   System.out.println(user==user2);

   session.close();
}
```

5、结果分析

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7KickRVspms8t4ZU0jXovPT2qe5QluO0MoibU09bTKiaGG923AzFwOSxICrM7BZFWNJqiaCUOGxDA54Tg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



> 一级缓存失效的四种情况

一级缓存是SqlSession级别的缓存，是一直开启的，我们关闭不了它；

一级缓存失效情况：没有使用到当前的一级缓存，效果就是，还需要再向数据库中发起一次查询请求！

1、sqlSession不同

```java
@Test
public void testQueryUserById(){
   SqlSession session = MybatisUtils.getSession();
   SqlSession session2 = MybatisUtils.getSession();
   UserMapper mapper = session.getMapper(UserMapper.class);
   UserMapper mapper2 = session2.getMapper(UserMapper.class);

   User user = mapper.queryUserById(1);
   System.out.println(user);
   User user2 = mapper2.queryUserById(1);
   System.out.println(user2);
   System.out.println(user==user2);

   session.close();
   session2.close();
}
```

观察结果：发现发送了两条SQL语句！

结论：**每个sqlSession中的缓存相互独立**

2、sqlSession相同，查询条件不同

```java
@Test
public void testQueryUserById(){
   SqlSession session = MybatisUtils.getSession();
   UserMapper mapper = session.getMapper(UserMapper.class);
   UserMapper mapper2 = session.getMapper(UserMapper.class);

   User user = mapper.queryUserById(1);
   System.out.println(user);
   User user2 = mapper2.queryUserById(2);
   System.out.println(user2);
   System.out.println(user==user2);

   session.close();
}
```

观察结果：发现发送了两条SQL语句！很正常的理解

结论：**当前缓存中，不存在这个数据**

3、sqlSession相同，两次查询之间执行了增删改操作！

增加方法

```java
//修改用户
int updateUser(Map map);
```

编写SQL

```sql
<update id="updateUser" parameterType="map">
  update user set name = #{name} where id = #{id}
</update>
```

测试

```java
@Test
public void testQueryUserById(){
   SqlSession session = MybatisUtils.getSession();
   UserMapper mapper = session.getMapper(UserMapper.class);

   User user = mapper.queryUserById(1);
   System.out.println(user);

   HashMap map = new HashMap();
   map.put("name","kuangshen");
   map.put("id",4);
   mapper.updateUser(map);

   User user2 = mapper.queryUserById(1);
   System.out.println(user2);

   System.out.println(user==user2);

   session.close();
}
```

观察结果：查询在中间执行了增删改操作后，重新执行了

结论：**因为增删改操作可能会对当前数据产生影响**

4、sqlSession相同，手动清除一级缓存

```java
@Test
public void testQueryUserById(){
   SqlSession session = MybatisUtils.getSession();
   UserMapper mapper = session.getMapper(UserMapper.class);

   User user = mapper.queryUserById(1);
   System.out.println(user);

   session.clearCache();//手动清除缓存

   User user2 = mapper.queryUserById(1);
   System.out.println(user2);

   System.out.println(user==user2);

   session.close();
}
```

一级缓存就是一个map



### 13.4、二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存

- 基于namespace级别的缓存，一个名称空间，对应一个二级缓存；

- 工作机制

- - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中；
  - 如果当前会话关闭了，这个会话对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中；
  - 新的会话查询信息，就可以从二级缓存中获取内容；
  - 不同的mapper查出的数据会放在自己对应的缓存（map）中；



> 使用步骤

1、开启全局缓存 【mybatis-config.xml】

```xml
<setting name="cacheEnabled" value="true"/>
```

2、去每个mapper.xml中配置使用二级缓存，这个配置非常简单；【xxxMapper.xml】

```xml
<cache/>

官方示例=====>查看官方文档
<cache
 eviction="FIFO"
 flushInterval="60000"
 size="512"
 readOnly="true"/>
这个更高级的配置创建了一个 FIFO 缓存，每隔 60 秒刷新，最多可以存储结果对象或列表的 512 个引用，而且返回的对象被认为是只读的，因此对它们进行修改可能会在不同线程中的调用者产生冲突。
```

3、代码测试

- 所有的实体类先实现序列化接口
- 测试代码

```java
@Test
public void testQueryUserById(){
   SqlSession session = MybatisUtils.getSession();
   SqlSession session2 = MybatisUtils.getSession();

   UserMapper mapper = session.getMapper(UserMapper.class);
   UserMapper mapper2 = session2.getMapper(UserMapper.class);

   User user = mapper.queryUserById(3);
   System.out.println(user);
   session.close();

   User user2 = mapper2.queryUserById(3);
   System.out.println(user2);
   System.out.println(user==user2);

   session2.close();
}
```

> 结论

- 只要开启了二级缓存，我们在同一个Mapper中的查询，可以在二级缓存中拿到数据
- 查出的数据都会被默认先放在一级缓存中
- 只有会话提交或者关闭以后，一级缓存中的数据才会转到二级缓存中



> 缓存原理图



![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7KickRVspms8t4ZU0jXovPT2egdNicaJuVnzMYxibyYFvB0COWW4sgDhHPqvFbG9F9KS1vX7ibIMNqefg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> EhCache



![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7KickRVspms8t4ZU0jXovPT29RfsqgE50IOicMbHPzceX3r2BWn2U9MZ29rpQAy3gtQBnWpveiaRr2lw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

第三方缓存实现--EhCache: 查看百度百科

Ehcache是一种广泛使用的java分布式缓存，用于通用缓存；

要在应用程序中使用Ehcache，需要引入依赖的jar包

```
<!-- https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-ehcache -->
<dependency>
   <groupId>org.mybatis.caches</groupId>
   <artifactId>mybatis-ehcache</artifactId>
   <version>1.1.0</version>
</dependency>
```

在mapper.xml中使用对应的缓存即可

```xml
<mapper namespace = “org.acme.FooMapper” >
   <cache type = “org.mybatis.caches.ehcache.EhcacheCache” />
</mapper>
```

编写ehcache.xml文件，如果在加载时未找到/ehcache.xml资源或出现问题，则将使用默认配置。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
        updateCheck="false">
   <!--
      diskStore：为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位置。参数解释如下：
      user.home – 用户主目录
      user.dir – 用户当前工作目录
      java.io.tmpdir – 默认临时文件路径
    -->
   <diskStore path="./tmpdir/Tmp_EhCache"/>
   
   <defaultCache
           eternal="false"
           maxElementsInMemory="10000"
           overflowToDisk="false"
           diskPersistent="false"
           timeToIdleSeconds="1800"
           timeToLiveSeconds="259200"
           memoryStoreEvictionPolicy="LRU"/>

   <cache
           name="cloud_user"
           eternal="false"
           maxElementsInMemory="5000"
           overflowToDisk="false"
           diskPersistent="false"
           timeToIdleSeconds="1800"
           timeToLiveSeconds="1800"
           memoryStoreEvictionPolicy="LRU"/>
   <!--
      defaultCache：默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
    -->
   <!--
     name:缓存名称。
     maxElementsInMemory:缓存最大数目
     maxElementsOnDisk：硬盘最大缓存个数。
     eternal:对象是否永久有效，一但设置了，timeout将不起作用。
     overflowToDisk:是否保存到磁盘，当系统当机时
     timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
     timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
     diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
     diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
     diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
     memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
     clearOnFlush：内存数量最大时是否清除。
     memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
     FIFO，first in first out，这个是大家最熟的，先进先出。
     LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
     LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
  -->

</ehcache>
```

