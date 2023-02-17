# Mybatis-plus学习笔记

## 1、创建数据库及表

### 1.1、创建表

```mysql
CREATE DATABASE `mybatis_plus`;

USE `mybatis_plus`;

CREATE TABLE `user` 
(
		`id` BIGINT(20) NOT NULL COMMENT '主键ID',
		`name` VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
		`age` INT(11) NULL DEFAULT NULL COMMENT '年龄',
		`email` VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
		PRIMARY KEY (`id`)
		
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

### 1.2、添加数据

```mysql
DELETE FROM user;
INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```

## 2、创建Spring Boot工程

### 2.1、初始化工程

**使用Spring Initializr 快速初始化一个Spring Boot 工程**

**MySQL、lombok、mybatis-plus**依赖

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <!--mybatis-plus启动器-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.1</version>
        </dependency>

        <!--mysql驱动器-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <!--lombok依赖用于简化实体类开发-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
```

**安装lombok插件**
`File——>Settings——>Plugins 搜lombok`

### 2.2、配置application.yml(application.properties)

**application.yml**

```yaml
spring:
  # 配置数据源
  datasource:
    # 数据源类型
    type: com.zaxxer.hikari.HikariDataSource
    # 配置连接数据库的各个信息
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatis_plus?characterEncoding=utf-8&useSSL=false&serverTimezone=GMT2%B8
    username: root
    password: ****
```

**application.properties**

```properties
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=****
```

## 3、创建实体类及lombok的简单使用

### 3.1、创建实体类

```java
@Data
public class User {

    private long id;
    
    private String name;
    
    private Integer age;
    
    private String email;
    
}
```

### 3.2、创建mapper接口

```java
@Repository
public interface UserMapper extends BaseMapper<User> {

}
```

**在启动类上添加@MapperScan**

```java
@SpringBootApplication
// 扫描mapper所在的包
@MapperScan("com.chen.mapper")
public class MybatisPlusApplication {
    public static void main(String[] args) {
        SpringApplication.run(MybatisPlusApplication.class, args);
    }

}
```

### 3.3、编写测试类

**MybatisPlusTest**

```java
@SpringBootTest
public class MybatisPlusTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void testSelectList() {
        // 通过条件构造器查询一个list集合，若有条件，则可以设置为null
        List<User> list = userMapper.selectList(null);
        list.forEach(System.out::println);
    }
}
```

### 3.4、加入日志功能

**在application.yml中加入以下代码**

```yaml
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

运行之后，可以查看生成SQL语句，也可以使用`Log4jImpl`对日志进行打印

## 4、BaseMapper

### 4.1、BaseMapper的添加方法测试

```java
@Test
public void testInsert() {
    // 实现新增用户信息
    // INSERT INTO user ( id, name, age, email ) VALUES ( ?, ?, ?, ? )
	User user = new User();
    user.setName("张三");
    user.setAge(18);
    user.setEmail("zhangsan@atguigu.com");
    int result = userMapper.insert(user);
    System.out.println("result:" + result);
    System.out.println("id:" + user.getId());
}
```

### 4.2、BaseMapper的删除方法测试

```java
@Test
public void testDelete() {
	// 通过ID删除一个用户
    // DELETE FROM user WHERE id=?
    /*int result = userMapper.deleteById(1499047219186933761L);
    System.out.println(result);*/

    // 根据map集合中的所设置的条件删除用户信息
    // DELETE FROM user WHERE name = ? AND age = ?
    /*Map<String, Object> map = new HashMap<>();
    map.put("name","张三");
    map.put("age",18);
    int result = userMapper.deleteByMap(map);
    System.out.println(result);*/


    // 通过多个id实现批量删除
    // DELETE FROM user WHERE id IN ( ? , ? )
    List<Long> list = Arrays.asList(1499047368323870722L, 1499047451270430721L);
    int result = userMapper.deleteBatchIds(list);
    System.out.println(result);
}
```

### 4.3、BaseMapper的更新方法测试

```java
@Test
public void testUpdate() {
	// 通过id来更新用户的信息
    // UPDATE user SET name=?, age=? WHERE id=?
    User user = new User();
    user.setId(1499050741030797314L);
    user.setName("张三");
    user.setAge(23);
    int result = userMapper.updateById(user);
    System.out.println(result);
}
```

### 4.4、BaseMapper的查询方法测试

```java
@Test
public void testSelect() {
	// 通过id查询用户信息
    // SELECT id,name,age,email FROM user WHERE id=?
    User user = userMapper.selectById(1L);
    System.out.println(user);

    // 通过多个id实现批量查询用户的信息
    // SELECT id,name,age,email FROM user WHERE id IN ( ? , ? , ? )
    List<Long> list = Arrays.asList(1L, 2L, 3L);
    List<User> userList = userMapper.selectBatchIds(list);
    userList.forEach(System.out::println);

    // 根据map集合中的条件来查询用户信息
    // SELECT id,name,age,email FROM user WHERE id = ? AND age = ?
    Map<String, Object> map = new HashMap<>();
    map.put("id",1L);
    map.put("age",18);
    List<User> users = userMapper.selectByMap(map);
    users.forEach(System.out::println);

    // 查询所有数据的
    // SELECT id,name,age,email FROM user
    List<User> userList = userMapper.selectList(null);
    userList.forEach(System.out::println);
}
```

### 4.5、自定义功能

**Mapper**

```java
@Repository
public interface UserMapper extends BaseMapper<User> {

    /**
     * 根据id查询用户信息为map集合
     * @param id
     * @return
     */
    Map<String, Object> selectMapById(Long id);

}
```

**UserMapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.chen.mapper.UserMapper">
    <select id="selectMapById" resultType="java.util.Map" parameterType="java.lang.Long">
        SELECT id,name,age,email FROM mybatis_plus.user WHERE id = #{id}
    </select>
</mapper>
```

**Test**

```java
@Test
public void selectMapById() {
	Map<String, Object> map = userMapper.selectMapById(1L);
    System.out.println(map);
}
```

## 5、通用Service接口

说明:

- 通用 Service CRUD 封装[IService (opens new window)](https://gitee.com/baomidou/mybatis-plus/blob/3.0/mybatis-plus-extension/src/main/java/com/baomidou/mybatisplus/extension/service/IService.java)接口，进一步封装 CRUD 采用 `get 查询单行` `remove 删除` `list 查询集合` `page 分页` 前缀命名方式区分 `Mapper` 层避免混淆，
- 泛型 `T` 为任意实体对象
- 建议如果存在自定义通用 Service 方法的可能，请创建自己的 `IBaseService` 继承 `Mybatis-Plus` 提供的基类
- 对象 `Wrapper` 为 [条件构造器](https://baomidou.com/01.指南/02.核心功能/wrapper.html)

- 官网地址：https://baomidou.com/pages/49cc81/#service-crud-%E6%8E%A5%E5%8F%A3

### 5.1、自定义的Service接口

```java
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {

}

// Service 接口
public interface UserService extends IService<User> {
}
```

**测试Service接口查询总数**

```java
@SpringBootTest
public class MybatisPlusServiceTest {

    @Autowired
    private UserService userService;
    
    @Test
    public void testGetCount() {
        // 查询总记录数的
        // SELECT COUNT( * ) FROM user
        long count = userService.count();
        System.out.println(count);
    }

}
```

**Service测试批量添加数据**

```java
@Test
public void testInsertMore() {
	// Service接口实现批量添加
	// INSERT INTO user ( id, name, age ) VALUES ( ?, ?, ? )
	List<User> list = new ArrayList<>();
	for (int i = 0; i <= 10; i++) {
		User user = new User();
		user.setName("ch" + i);
		user.setAge(20 + i);
		list.add(user);
    }

    boolean b = userService.saveBatch(list);
    System.out.println(b);
}
```

### 5.2、常用注解

### 5.2.1、@TableName

```java
// 设置实体类所对应的表名
@TableName("t_user")
```

```yaml
mybatis-plus:
# 设置Mybatis-plus的全局配置
  	global-config:
    	db-config:
     	 # 设置实体类所对应的标的统一前缀
      		table-prefix: t_

```

### 5.2.2、TableId注解

**将属性所对应字段作为主键**

```java
@TableId
// 将uid作为主键
private Long uid;
```

**TableId的value属性**

```java
@TableId(value = "uid")
// 将uid作为主键
private Long uid;
```

**TableId的type属性**

- 可以设置主键生成策略

```java
@TableId(value = "uid", type = IdType.AUTO)
// 将uid作为主键
private Long uid;
```

**全局配置主键生成策略**

```java
# 设置Mybatis-plus的全局配置
  global-config:
    db-config:
      # 设置统一的主键生成策略
      id-type: auto
```

**雪花算法**

snowflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。可以保证几乎全球唯一！

**其余源码的解释**

```java
public enum IdType {
    AUTO(0), // 数据库id自增
    NONE(1), // 未设置主键
    INPUT(2), // 手动输入，自己写id
    ID_WORKER(3), // 默认的全局唯一id
    UUID(4), // 全局唯一id uuid
    ID_WORKER_STR(5); // ID_WORKER 字符串表示法
}
```

### 5.2.3、TableField注解

**在数据库字段名和自己定义的字段名不一致时可以使用该注解**

```java
// 指定属性所对应的字段名
@TableField("name")
private String name;
```

### 5.2.4、TableLogic注解

可以实现对数据库中的数据进行逻辑删除，可以进行数据恢复

- 在数据库中添加字段is deleted

```java
@TableLogic
private Integer isDeleted;
```

## 6、条件构造器

### 6.1、wrapper介绍

在学习`Wapper`之前，先来看一下它的类图结构。

类图关键类说明：

- Wrapper ： 条件构造抽象类，最顶端父类
  - AbstractWrapper ： 用于查询条件封装，生成 sql 的 where 条件
    - QueryWrapper ： Entity 对象封装操作类，不是用lambda语法
    - UpdateWrapper ： Update 条件封装，用于Entity对象更新操作
    - AbstractLambdaWrapper ： Lambda 语法使用 Wrapper统一处理解析 lambda 获取 column。
      - LambdaQueryWrapper ：看名称也能明白就是用于Lambda语法使用的查询Wrapper
      - LambdaUpdateWrapper ： Lambda 更新封装Wrapper

### 6.2、QueryWrapper条件构造器

**组装条件查询并测试**

```java
@Test
public void testSelectWrapper() {
	// 查询用户名包含a,年龄在20到30之间，邮箱信息不为null的用户信息
    // SELECT id,name,age,email FROM user WHERE (name LIKE ? AND age BETWEEN ? AND ? AND email IS NOT NULL)
	QueryWrapper<User> queryWrapper = new QueryWrapper<>();
	queryWrapper.like("name","a")
                 .between("age", 20, 30)
                 .isNotNull("email");
    List<User> userList = userMapper.selectList(queryWrapper);
    userList.forEach(System.out::println);
}
```

**组装排序条件并测试**

```java
@Test
public void test02() {
    // 查询用户信息，按照年龄的降序排序，若年龄相同，则按照id升序排序
    // SELECT id,name,age,email FROM user ORDER BY age DESC,id ASC
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    queryWrapper.orderByDesc("age")
                .orderByAsc("id");
    List<User> list = userMapper.selectList(queryWrapper);
    list.forEach(System.out::println);
}
```

**组装删除条件并测试**

```java
@Test
public void test03() {
    // 删除邮箱地址null的用户信息
    // DELETE FROM user WHERE (email IS NULL)
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    queryWrapper.isNull("email");
    int i = userMapper.delete(queryWrapper);
    System.out.println(i);
}
```

**组装修改功能并测试**

```java
@Test
public void test04() {
    // 将（年龄大于20并且用户名中包含有a）或邮箱为null的用户信息修改
    // UPDATE user SET name=?, email=? WHERE (age > ? AND name LIKE ? OR email IS NULL)
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    queryWrapper.gt("age", 20)
            .like("name", "a")
            .or()
            .isNull("email");
    User user = new User();
    user.setName("小明");
    user.setEmail("test@atguigu.com");
    int result = userMapper.update(user, queryWrapper);
    System.out.println(result);
}
```

**条件优先级的测试**

```java
@Test
public void test05() {
    // 将用户名中包含a并且（年龄大于20或邮箱为null）的用户信息修改
    // Lambda中的条件优先执行
    // UPDATE user SET name=?, email=? WHERE (name LIKE ? AND (age > ? OR email IS NULL))
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    queryWrapper.like("name", "a")
            .and(i -> i.gt("age", 20)
            .or()
            .isNull("email"));
    User user = new User();
    user.setName("小红");
    user.setEmail("tset@atguigu.com");
    int result = userMapper.update(user, queryWrapper);
    System.out.println(result);
}
```

**组装select语句**

```java
@Test
public void test06() {
    // 查询用户的用户名，年龄，邮箱信息
    // SELECT name,age,email FROM user
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    queryWrapper.select("name", "age", "email");
    List<Map<String, Object>> maps = userMapper.selectMaps(queryWrapper);
    maps.forEach(System.out::println);
}
```

**组装子查询并测试**

```java
@Test
public void test07() {
    // 查询id小于等于100的用户信息
    // SELECT id,name,age,email FROM user WHERE (id IN (select id from user where id <= 100))
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    queryWrapper.inSql("id", "select id from user where id <= 100");
    List<User> list = userMapper.selectList(queryWrapper);
    list.forEach(System.out::println);
}
```

**使用UpdateWrapper实现修改功能**

```java
@Test
public void test08() {
    // 将用户名中包含a并且（年龄大于20或邮箱为null）的用户信息修改
    // UPDATE user SET name=?,email=? WHERE (name LIKE ? AND (age > ? OR email IS NULL))
    UpdateWrapper<User> updateWrapper = new UpdateWrapper<>();
    updateWrapper.like("name", "a")
            .and(i -> i.gt("age", 20)
                    .or()
                    .isNull("email"));
    updateWrapper.set("name", "小黑");
    updateWrapper.set("email", "abc@atguigu.com");
    int result = userMapper.update(null, updateWrapper);
    System.out.println(result);
}
```

**模拟开发中的组装条件**

```java
@Test
public void test09() {
    String username = "";
    Integer ageBegin = 20;
    Integer ageEnd = 30;
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    if (StringUtils.isNotBlank(username)) {
        // 判断某个字符串是否不为空字符串，不为null，不为空白符
        // SELECT id,name,age,email FROM user WHERE (age >= ? AND age <= ?)
        queryWrapper.like("name", username);
    }

    if (ageBegin != null) {
        queryWrapper.ge("age", ageBegin);
    }
    if (ageEnd != null) {
        queryWrapper.le("age", ageEnd);

    }
    List<User> list = userMapper.selectList(queryWrapper);
    list.forEach(System.out::println);
} 
```

**使用condition动态组装条件**

```java
@Test
public void test() {
    String username = "a";
    Integer ageBegin = null;
    Integer ageEnd = 30;
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    queryWrapper.like(StringUtils.isNotBlank(username), "name", username)
            .ge(ageBegin != null, "age", ageBegin)
            .lt(ageEnd != null, "age", ageEnd);
    List<User> list = userMapper.selectList(queryWrapper);
    list.forEach(System.out::println);

}
```

**LambdaQueryWrapper**

```java
@Test
public void test11() {
    // SELECT id,name,age,email FROM user WHERE (name LIKE ? AND age < ?)
    String username = "a";
    Integer ageBegin = null;
    Integer ageEnd = 30;

    LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
    queryWrapper.like(StringUtils.isNotBlank(username), User::getName, username)
            .ge(ageBegin != null, User::getAge, ageBegin)
            .lt(ageEnd != null, User::getAge, ageEnd);
    List<User> list = userMapper.selectList(queryWrapper);
    list.forEach(System.out::println);
}
```

**LambdaUpdateWrapper**

```java
@Test
public void test12() {
    // 将用户名中包含a并且（年龄大于20或邮箱为null）的用户信息修改
    LambdaUpdateWrapper<User> updateWrapper = new LambdaUpdateWrapper<>();
    updateWrapper.like(User::getName, "a")
            .and(i -> i.gt(User::getAge, 20)
                    .or()
                    .isNull(User::getEmail));
    updateWrapper.set(User::getName, "小黑").set(User::getName, "abc@atguigu.com");
    int result = userMapper.update(null, updateWrapper);
    System.out.println(result);
}
```

## 7、插件

### 7.1、分页插件

> Mybatis-plus自带分页插件，自己需要简单的配置计科实现分页功能

**添加配置**

- 可以把启动类上的扫描mapper的注解放在MyBatisPlusConfig上

```java
@Configuration
// 扫描mapper所在的包
@MapperScan("com.chen.mapper")
public class MyBatisPlusConfig {

    @Bean
    // 配置MybatisPlus中的插件
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }

}    
@Test
public void testPage() {
    Page<User> page = new Page<>(1, 3);
    userMapper.selectPage(page, null);
    System.out.println(page);
}
```

**获取分页相关的数据**

```java
@Test
public void testPage() {
    Page<User> page = new Page<>(2, 3);
    userMapper.selectPage(page, null);
    System.out.println(page.getRecords());
    System.out.println(page.getPages());
    System.out.println(page.getTotal());
    System.out.println(page.hasNext());
    System.out.println(page.hasPrevious());
}
```

**自定义的分页功能**

```java
/**
 * 通过年龄查询用户信息并分页
 * @param page  Mybatis-Plus所提供的分页对象，必须位于第一个参数的位置
 * @param age   参数值
 * @return  返回值
 */
Page<User> selectPageVo(@Param("page") Page<User> page, @Param("age") Integer age);
```

```xml
<select id="selectPageVo" resultType="com.chen.pojo.User">
     SELECT id, name, age, email FROM mybatis_plus.user WHERE age > #{age}
</select>
```

```java
@Test
public void testPageVo() {
    Page<User> page = new Page<>();
    userMapper.selectPageVo(page, 20);
    System.out.println(page.getRecords());
    System.out.println(page.getPages());
    System.out.println(page.getTotal());
    System.out.println(page.hasNext());
    System.out.println(page.hasPrevious());
}
```

### 6.2、乐观锁

### 6.2.1、模拟修改冲突

#### 数据库中增加商品表

```sql
CREATE TABLE product
(
    id BIGINT(20) NOT NULL COMMENT '主键id',
    NAME VARCHAR(30) NULL DEFAULT NULL COMMENT '商品名称',
    price INT(11) DEFAULT 0 COMMENT '价格',
    VERSION INT(11) DEFAULT 0 COMMENT '乐观锁版本号',
    PRIMARY KEY (id)
);
```

### 添加数据

```sql
INSERT INTO t_product (id, NAME, price) VALUES (1, '外星人', 10000);
```

### 创建实体类

```java
package com.chen.pojo;

import lombok.Data;

@Data
public class Product {

    private Long id;

    private String name;

    private Integer price;

    private Integer version;

}
```

### 添加Mapper

```java
@Repository
// 持久化注解
public interface ProductMapper extends BaseMapper<Product> {

}
```

### 模拟修改冲突问题

```java
@Test
public void testProduct01() {
    // 小李查询商品价格
    Product productLi = productMapper.selectById(1);
    System.out.println("小李查询的商品价格：" + productLi.getPrice());

    // 小王查询商品价格
    Product productWang = productMapper.selectById(1);
    System.out.println("小王查询的商品价格：" + productWang.getPrice());

    // 小李将商品价格+50
    productLi.setPrice(productLi.getPrice() + 50);
    productMapper.updateById(productLi);

    // 小王将商品的价格-30
    productWang.setPrice(productWang.getPrice() - 30);
    productMapper.updateById(productWang);

    // 老板查询商品价格
    Product productBOSS = productMapper.selectById(1);
    System.out.println("老板查询的商品价格：" + productBOSS.getPrice());
}
```



### 乐观锁的实现流程

取出记录是，获取当前的version

```sql
SELECT id, `name`, price, `version` FROM product WHERE id = 1
```

更新时，version+1,如果where语句中的 version版本不对，则更新失败

```sql
UPDATE product SET price = price + 50, `version` = `version` + 1 WHERE id = 1 AND `version` = 1;
```

### Mybatis-plus实现乐观锁

#### 修改实体类

```java
@Data
public class Product {

    private Long id;

    private String name;

    private Integer price;

    // 表示乐观锁版本号字段
    @Version
    private Integer version;

}
```

#### 在配置文件中添加乐观锁插件

```java
// 添加乐观锁插件
interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
```

#### 重新进行测试

```java
@Test
public void testProduct01() {
    // 小李查询商品价格
    Product productLi = productMapper.selectById(1);
    System.out.println("小李查询的商品价格：" + productLi.getPrice());

    // 小王查询商品价格
    Product productWang = productMapper.selectById(1);
    System.out.println("小王查询的商品价格：" + productWang.getPrice());

    // 小李将商品价格+50
    productLi.setPrice(productLi.getPrice() + 50);
    productMapper.updateById(productLi);

    // 小王将商品的价格-30
    productWang.setPrice(productWang.getPrice() - 30);
    int result = productMapper.updateById(productWang);
    if (result == 0) {
        // 操作失败，重试
        Product productNew = productMapper.selectById(1);
        productNew.setPrice(productNew.getPrice() - 30);
        productMapper.updateById(productNew);

    }

    // 老板查询商品价格
    Product productBoss = productMapper.selectById(1);
    System.out.println("老板查询的商品价格：" + productBoss.getPrice());
}
```

## 8、通用枚举

> 表中有些字段的值是固定的，例如性别（男女），此时我们可以使用Mybatis-plus的通用枚举来实现

### 数据库添加字段sex

### 在实体类中添加属性

```java
private SexEnum sex;
```

### 创建枚举类型

```java
@Getter
public enum SexEnum {
    /**
     *
     */
    MALE(1, "男"),
    FEMALE(2, "女");

    // 将注解所标识的属性值保存到数据库中
    @EnumValue
    private Integer sex;

    private String sexname;


    SexEnum(Integer sex, String sexname) {
        this.sex = sex;
        this.sexname = sexname;
    }
}
```

### 创建测试类

```java
@SpringBootTest
public class MybatisPlusEnumTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void test() {
        User user = new User();
        user.setName("admin");
        user.setAge(33);
        user.setSex(SexEnum.MALE);
        int insert = userMapper.insert(user);
        System.out.println(insert);
    }

}
```

**需要在配置文件中配置通用枚举的扫描包**

```yaml
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  # 设置Mybatis-plus的全局配置
  global-config:
    db-config:
      # 设置实体类所对应的标的统一前缀
      # table-prefix: t_
      # 设置统一的主键生成策略
      id-type: auto
  # 配置类型别名所对应的包
  type-aliases-package: com.chen.pojo
  # 扫描通用枚举的包
  type-enums-package: com.chen.enums
```

## 9、代码生成器

### 9.1、引入依赖

```xml
<!--代码生成器的依赖-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.4.1</version>
</dependency>
<dependency>
    <groupId>org.freemarker</groupId>
    <artifactId>freemarker</artifactId>
</dependency>
```

### 9.2、快速生成

```java
public class FastAutoGeneratorTest {
    public static void main(String[] args) {
        FastAutoGenerator.create("jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&serverTimezone=GMT%2B8",
                "root", "root")
                .globalConfig(builder -> {
                    builder.author("chen") // 设置作者
                            //.enableSwagger() // 开启 swagger 模式
                            .fileOverride() // 覆盖已生成文件
                            .outputDir("D://mybatis-plus"); // 指定输出目录
                })
                .packageConfig(builder -> {
                    builder.parent("com.chen") // 设置父包名
                            .moduleName("mybatis-plus") // 设置父包模块名
                            .pathInfo(Collections.singletonMap(OutputFile.mapperXml, "D://mybatis-plus")); // 设置mapperXml生成路径
                })
                .strategyConfig(builder -> {
                    builder.addInclude("user") // 设置需要生成的表名
                            .addTablePrefix("t_", "c_"); // 设置过滤表前缀
                })
                .templateEngine(new FreemarkerTemplateEngine()) // 使用Freemarker引擎模板，默认的是Velocity引擎模板
                .execute();
    }

}
```

## 10、多数据源

### 10.1、创建数据库及表

> 创建数据库mybatis_plus_1和表product

```sql
CREATE DATABASE mybatis_plus_1;

USE mybatis_plus_1;


CREATE TABLE product
(
    id BIGINT(20) NOT NULL COMMENT '主键id',
    NAME VARCHAR(30) NULL DEFAULT NULL COMMENT '商品名称',
    price INT(11) DEFAULT 0 COMMENT '价格',
    VERSION INT(11) DEFAULT 0 COMMENT '乐观锁版本号',
    PRIMARY KEY (id)
);

```

> 添加测试数据

```sql
INSERT INTO product (id, NAME, price) VALUES (1, '外星人', 100);
```

> 删除mybatis_plus库和product表

```sql
USE mybatis_plus;
DROP TABLE IF EXISTS product;
```

### 10.2、引入依赖

```xml
<dependency>
	<groupId>com.baomidou</groupId>
    <artifactId>dynamic-datasource-spring-boot-starter</artifactId>
    <version>3.5.0</version>
</dependency>
```

### 10.3、配置多数据源

> 说明：注释掉之前的数据库连接，添加新配置

```yaml
spring:
  # 配置数据源信息
  datasource:
    dynamic:
      # 设置默认的数据源或数据源组，默认值即为master
      primary: master
      # 严格匹配数据源，默认false，true未匹配到指定数据源时抛异常，false使用默认数据源
      strict: false
      datasource:
        master:
          url: jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&serverTimezone=GMT%2B8
          driver-class-name: com.mysql.cj.jdbc.Driver
          username: root
          password: root
        save_1:
          url: jdbc:mysql://localhost:3306/mybatis_plus_1?useSSL=false&serverTimezone=GMT%2B8
          driver-class-name: com.mysql.cj.jdbc.Driver
          username: root
          password: root
```

### 10.4、创建用户service

```java
public interface UserService extends IService<User> {
}
```

```java
@DS("master")
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {
}
```

### 10.5、创建商品service

```java
public interface ProductService extends IService<Product> {
}
```

```java
@DS("slave_1")
@Service
public class ProductServiceImpl extends ServiceImpl<ProductMapper, Product> implements ProductService {
}
```

### 10.6、测试

```java
@Autowired
private UserService userService;

@Autowired
private ProductService productService;


@Test
public void test() {
    System.out.println(userService.getById(1));
    System.out.println(productService.getById(1));

}
```

## 11、MybatisX插件

### 安装

MybatisX插件的用法：https://baomidou.com/pages/ba5b24
