# 快速开始

我们将通过一个简单的 Demo 来阐述 MyBatis-Plus 的强大功能，在此之前，我们假设您已经：

- 拥有 Java 开发环境以及相应 IDE
- 熟悉 Spring Boot
- 熟悉 Maven 或者 Gradle

---

现有一张 `User` 表，其表结构如下：

| id  | name   | age | email              |
| --- | ------ | --- | ------------------ |
| 1   | Jone   | 18  | test1@baomidou.com |
| 2   | Jack   | 20  | test2@baomidou.com |
| 3   | Tom    | 28  | test3@baomidou.com |
| 4   | Sandy  | 21  | test4@baomidou.com |
| 5   | Billie | 24  | test5@baomidou.com |

::: danger Question
如果从零开始用 MyBatis-Plus 来实现该表的增删改查我们需要做什么呢？
:::

## 初始化工程

创建一个空的 Spring Boot 工程，配置数据库连接（Demo 工程采用 H2 作为测试数据库）

::: tip
可以使用 [Spring Initializr](https://start.spring.io/) 快速初始化一个 Spring Boot 工程
:::

在 `application.yml` 配置文件中添加 H2 数据库相关配置：

```yaml
# DataSource Config
spring:
  datasource:
    driver-class-name: org.h2.Driver
    schema: classpath:db/schema-h2.sql
    data: classpath:db/data-h2.sql
    url: jdbc:h2:mem:test
    username: root
    password: test
```

数据库 Schema 脚本：

```sql
DROP TABLE IF EXISTS user;

CREATE TABLE user
(
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY (id)
);
```

数据库 Data 脚本：

```sql
DELETE FROM user;

INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```

## 添加依赖

引入 `mybatis-plus-boot-starter` 依赖

Maven:
```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>2.3</version>
</dependency>
```

Gradle:
```gradle
compile('com.baomidou:mybatis-plus-boot-starter:2.3')
```

## 配置

在 Spring Boot 启动类中添加 `@MapperScan` 注解：
```java {2}
@SpringBootApplication
@MapperScan("com.baomidou.mybatisplus.springbootsample.mapper")
public class SpringBootSampleApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootSampleApplication.class, args);
    }

}
```

## 编码

编写实体类 `User.java`（此处使用了 [Lombok](https://www.projectlombok.org/) 简化代码）

```java
@Data
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

编写Mapper类 `UserMapper.java`

```java
public interface UserMapper extends BaseMapper<User> {

}
```

## 开始使用

添加测试类，进行功能测试：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringBootSampleApplicationTests {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void testSelectAll() {
        List<User> userList = userMapper.selectList(null);
        userList.forEach(System.out::println);
    }

}
```

::: tip 温馨提示
UserMapper 中的 `selectList()` 方法的参数为 MP 内置的条件封装器 `Wrapper`，所以不填写就是无任何条件
:::

控制台输出：

```
User(id=1, name=Jone, age=18, email=test1@baomidou.com)
User(id=2, name=Jack, age=20, email=test2@baomidou.com)
User(id=3, name=Tom, age=28, email=test3@baomidou.com)
User(id=4, name=Sandy, age=21, email=test4@baomidou.com)
User(id=5, name=Billie, age=24, email=test5@baomidou.com)
```

通过以上几个简单的步骤，我们就实现了 User 表的 CRUD 功能，甚至连 XML 文件都不用编写！但 MyBatis-Plus 的强大远不止这些功能，我们还有强大的代码生成器，通过简单配置即可一键生成 Entity、Mapper、Service 等模块代码，节省大量时间。

想要详细了解 MyBatis-Plus 的强大功能？那就继续往下看吧！