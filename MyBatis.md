### 入手
#### 1.添加依赖
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>...</version> </dependency>

    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <scope>runtime</scope>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```
- **`mybatis-spring-boot-starter`**: 这是关键！它会自动配置 MyBatis 所需的核心对象，如 `SqlSessionFactory` 和 `SqlSessionTemplate`，并整合 Spring 的数据源和事务管理。
- **`mysql-connector-j`**: MySQL 数据库驱动。
- **`lombok`**: (推荐) 可以用 `@Data`, `@Getter`, `@Setter` 等注解简化 JavaBean 的编写。
#### 2.配置数据源
```properties
# application.properties 示例
spring.datasource.url=jdbc:mysql://localhost:3306/your_database_name?useSSL=false&serverTimezone=UTC&characterEncoding=utf8
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver # 注意 MySQL 8+ 的驱动类名

# MyBatis 配置 (可选， starter 有默认值)
mybatis.mapper-locations=classpath:mapper/**/*.xml # 指定 Mapper XML 文件位置
mybatis.type-aliases-package=com.yourcompany.yourproject.domain # 指定实体类的包，可以在 XML 中使用类名简称
# mybatis.configuration.map-underscore-to-camel-case=true # 自动将数据库下划线命名映射到 Java 驼峰命名
```
- 配置数据库连接信息。
- `mybatis.mapper-locations`: 告诉 MyBatis 去哪里查找你的 SQL 映射文件 (XML)。
- `mybatis.type-aliases-package`: 定义实体类所在的包，这样在 XML 中引用 `com.yourcompany.yourproject.domain.User` 时，可以直接写 `User`。
- `map-underscore-to-camel-case`: 非常常用的配置，自动处理数据库 `user_name` 到 Java `userName` 的映射。

#### 3. 创建实体类 (POJO)
```java
package com.yourcompany.yourproject.domain;

import lombok.Data; // 使用 Lombok (可选)
import java.time.LocalDateTime;

@Data // Lombok 注解，自动生成 getter/setter/toString等
public class User {
    private Long id;
    private String username;
    private String password;
    private String email;
    private LocalDateTime createdAt;
}
```

#### 4. 创建 Mapper 接口
```java
package com.yourcompany.yourproject.mapper;

import com.yourcompany.yourproject.domain.User;
import org.apache.ibatis.annotations.Mapper; // 关键注解
import org.apache.ibatis.annotations.Param; // 用于多个参数传递

import java.util.List;

@Mapper // 标记这是一个 MyBatis Mapper 接口，Spring Boot 会扫描并为其创建代理实现
public interface UserMapper {

    // 根据 ID 查询用户
    User findById(Long id);

    // 查询所有用户
    List<User> findAll();

    // 插入用户 (返回影响的行数)
    int insert(User user);

    // 更新用户
    int update(User user);

    // 删除用户
    int deleteById(Long id);

    // 示例：根据用户名模糊查询 (演示参数传递)
    List<User> findByUsernameLike(@Param("username") String username); // 使用 @Param 指定参数名
}
```
- **`@Mapper`**: 核心注解！Spring Boot 通过这个注解找到 Mapper 接口并进行代理。
- 接口中的方法名通常与你要执行的操作相关。
- 方法的参数对应 SQL 语句中需要的参数。
- 方法的返回值对应 SQL 查询的结果。
- **`@Param`**: 当方法有多个参数时，建议使用 `@Param` 注解明确指定每个参数在 XML 中对应的名字，避免混淆。
#### 5.创建 Mapper XML 文件
- 在 `src/main/resources/mapper/` 目录下（根据 `mybatis.mapper-locations` 配置）创建对应的 XML 文件，例如 `UserMapper.xml`。
```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yourcompany.yourproject.mapper.UserMapper"> <resultMap id="BaseResultMap" type="com.yourcompany.yourproject.domain.User">
        <id property="id" column="id" /> <result property="username" column="username" />
        <result property="password" column="password" />
        <result property="email" column="email" />
         <result property="createdAt" column="created_at" />
    </resultMap>

    <select id="findById" resultType="com.yourcompany.yourproject.domain.User" parameterType="long">
        SELECT id, username, password, email, created_at
        FROM user
        WHERE id = #{id} </select>

    <select id="findAll" resultType="com.yourcompany.yourproject.domain.User">
        SELECT id, username, password, email, created_at FROM user
    </select>

    <insert id="insert" parameterType="com.yourcompany.yourproject.domain.User" useGeneratedKeys="true" keyProperty="id">
        INSERT INTO user (username, password, email, created_at)
        VALUES (#{username}, #{password}, #{email}, #{createdAt}) </insert>

    <update id="update" parameterType="com.yourcompany.yourproject.domain.User">
        UPDATE user
        SET username = #{username}, password = #{password}, email = #{email}
        WHERE id = #{id}
    </update>

    <delete id="deleteById" parameterType="long">
        DELETE FROM user WHERE id = #{id}
    </delete>

    <select id="findByUsernameLike" resultType="com.yourcompany.yourproject.domain.User">
      SELECT id, username, password, email, created_at
      FROM user
      WHERE username LIKE CONCAT('%', #{username}, '%') </select>

</mapper>
```
- **`<!DOCTYPE ...>`**: MyBatis Mapper XML 的标准 DTD 定义。
- **`<mapper namespace="...">`**: **极其重要！** namespace 必须指向对应的 Mapper 接口的全限定名。MyBatis 通过这个 namespace 将 XML 与接口关联起来。
- **`<select>`, `<insert>`, `<update>`, `<delete>`**: 定义不同类型的 SQL 操作。
    - `id`: 必须与 Mapper 接口中的方法名完全一致。
    - `resultType`/`resultMap`: 指定查询结果如何映射到 Java 对象。`resultType` 用于简单映射，`resultMap` 用于处理列名和属性名不一致或复杂关系（如一对多、多对一）的映射。如果开启了 `map-underscore-to-camel-case` 且命名符合规范，通常 `resultType` 就够用了。
    - `parameterType`: 指定输入参数的类型。对于简单类型（如 `long`, `String`）或单个 POJO，通常可以省略，MyBatis 会自动推断。
    - `#{...}`: **参数占位符**。MyBatis 会使用 PreparedStatement，这能防止 SQL 注入。它会获取传入参数对象（或 `@Param` 注解指定的参数）的相应属性值。
    - `${...}`: **字符串替换**。不推荐用于接收用户输入，因为它直接将变量内容拼接到 SQL 中，容易引发 SQL 注入。通常用于需要动态替换表名、列名等非参数化场景。
- **`useGeneratedKeys="true" keyProperty="id"`**: 在 `insert` 语句中常用，用于获取数据库生成的自增主键，并将其设置回传入的 `User` 对象的 `id` 属性。
#### 6. 在 Service 或 Controller 中注入并使用 Mapper
```java
package com.yourcompany.yourproject.service;

import com.yourcompany.yourproject.domain.User;
import com.yourcompany.yourproject.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional; // 导入事务注解

import java.util.List;

@Service // 标记为 Spring 服务类
public class UserService {

    // @Autowired // 字段注入 (不推荐)
    private final UserMapper userMapper; // 使用 final

    // 推荐使用构造器注入
    @Autowired
    public UserService(UserMapper userMapper) {
        this.userMapper = userMapper;
    }

    public User getUserById(Long id) {
        return userMapper.findById(id);
    }

    public List<User> getAllUsers() {
        return userMapper.findAll();
    }

    // 默认情况下，Spring Boot 对 JpaRepository 方法启用事务。
    // 对于自定义的 Service 方法，如果包含写操作（增删改），建议显式添加 @Transactional
    @Transactional // 开启事务
    public User createUser(User user) {
        user.setCreatedAt(java.time.LocalDateTime.now()); // 可以在这里设置创建时间
        userMapper.insert(user);
        // insert 方法配置了 useGeneratedKeys="true" keyProperty="id" 后，
        // user 对象的 id 属性会被自动填充上数据库生成的主键值
        return user; // 返回带有 ID 的 User 对象
    }

    @Transactional
    public boolean updateUser(User user) {
        return userMapper.update(user) > 0; // update 返回影响的行数
    }

    @Transactional
    public boolean deleteUser(Long id) {
        return userMapper.deleteById(id) > 0;
    }

    public List<User> findUsersByUsername(String username) {
        return userMapper.findByUsernameLike(username);
    }
}
```
- 使用 `@Autowired` 将 Spring Boot 自动创建的 `UserMapper` 代理对象注入到你的 Service 层。推荐使用构造器注入。
- 直接调用 Mapper 接口的方法，就像调用普通的 Java 方法一样。MyBatis 会在底层执行对应的 XML 中的 SQL 语句。
- 对于涉及写操作（INSERT, UPDATE, DELETE）的方法，建议在 Service 层方法上添加 `@Transactional` 注解，以确保操作的原子性。Spring Boot 会自动管理事务。
#### 7.启动配置类
```java
package com.yourcompany.yourproject;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("com.yourcompany.yourproject.mapper") // 明确指定扫描 Mapper 接口的包
public class YourApplication {

    public static void main(String[] args) {
        SpringApplication.run(YourApplication.class, args);
    }
}
```
- `@MapperScan`: 虽然 `@Mapper` 注解通常足够，但 `@MapperScan` 可以集中指定 Mapper 接口所在的包，避免在每个 Mapper 接口上都写 `@Mapper` 注解。
### 其他注意事项
1. **`SqlSessionFactory` & `SqlSession`**:
    - 在未使用 Spring Boot Starter 的情况下，你需要手动配置和创建 `SqlSessionFactory` (基于 `mybatis-config.xml`)。`SqlSessionFactory` 是重量级的，全局一个即可。
    - 通过 `SqlSessionFactory` 获取 `SqlSession`。`SqlSession` 类似于数据库连接，是**线程不安全**的，每次操作都应该获取新的 `SqlSession`，并在 `finally` 块中关闭它。
    - **Spring Boot Starter 的优势:** `mybatis-spring-boot-starter` 帮你管理了 `SqlSessionFactory` 的创建和 `SqlSession` 的生命周期 (通过 `SqlSessionTemplate`)，并将其与 Spring 的事务管理无缝集成。你只需要注入 Mapper 接口即可，无需关心这些底层细节。
2. **Mapper 接口与 XML 的关系**: 接口方法与 XML中的 `<select|insert|update|delete>` 元素的 `id` 属性一一对应。Namespace 指向接口。
3. **参数传递 (`#{}` vs `${}`)**: 永远优先使用 `#{}` 来传递参数值以防止 SQL 注入。仅在绝对必要（如动态表名/列名，且来源可信）时才考虑 `${}`。
4. **结果映射 (`resultType` vs `resultMap`)**: `resultType` 用于简单映射，`resultMap` 用于复杂映射（列名属性名不一致、关联查询等）。开启 `map-underscore-to-camel-case=true` 能解决大部分命名不一致问题。
5. **动态 SQL**: MyBatis 的强大之处在于动态 SQL (`<if>`, `<choose>`, `<when>`, `<otherwise>`, `<where>`, `<set>`, `<foreach>`)。它允许你根据条件动态地构建 SQL 语句，非常灵活。这是**必须掌握**的核心技能。
    
    - **例如:** 根据传入的参数决定是否添加某个查询条件。
    - 
    ```xml
    <select id="findActiveUsers" resultType="User">
        SELECT * FROM user
        <where> <if test="username != null and username != ''">
                AND username LIKE #{username}
            </if>
            <if test="email != null and email != ''">
                AND email = #{email}
            </if>
            AND status = 'ACTIVE'
        </where>
    </select>
    ```
    
6. **缓存 (一级缓存和二级缓存)**:
    - **一级缓存 (SqlSession 级别，默认开启)**: 在同一个 `SqlSession` 内，对同一个查询（相同 ID 和参数）的多次调用，只有第一次会查询数据库，后续会从缓存中获取。`SqlSession` 关闭或提交/回滚事务后，一级缓存失效。在 Spring Boot 集成下，每次方法调用通常对应一个新的 `SqlSession`，一级缓存的作用有限。
    - **二级缓存 (Mapper Namespace 级别，默认关闭)**: 多个 `SqlSession` 可以共享同一个 Mapper 的二级缓存。需要在 XML 中配置 `<cache/>` 开启。需要谨慎使用，因为它可能导致脏数据问题（如果其他地方绕过 MyBatis 直接修改了数据库）。通常用于读多写少的场景，并且需要对缓存策略有深入理解。
    - **未来趋势**: 分布式缓存（如 Redis, Memcached）通常是更常用、更可靠的缓存方案，与 MyBatis 结合使用。

**后续学习与进阶方向 (构建现代化知识体系)**

1. **深入掌握动态 SQL**: 这是 MyBatis 的精髓，务必熟练运用。
2. **学习 `ResultMap` 的高级用法**: 处理一对一、一对多、多对一的关联查询映射。
3. **探索 MyBatis 注解**: 除了 XML，也可以使用注解来编写 SQL，适合简单 SQL。但对于复杂 SQL 和动态 SQL，XML 通常更清晰、更易维护。了解即可，主流还是 XML。
    
    Java
    
    ```
    @Select("SELECT * FROM user WHERE id = #{id}")
    User findById(Long id);
    ```
    
4. **MyBatis Plus (重要!)**:
    - **是什么**: 一个为 MyBatis 量身定制的**增强工具**，在 MyBatis 的基础上只做增强不做改变。它提供了大量的便捷功能，极大地简化了开发，尤其是 CRUD 操作。
    - **为什么学**:
        - **大幅简化 CRUD**: 内置了通用的 Mapper 和 Service，实现了常见的单表增删改查方法，无需编写 XML 或 SQL 语句。
        - **强大的条件构造器 (Wrapper)**: 可以非常方便地以编程方式构造复杂的查询条件，替代部分动态 SQL。
        - **分页插件 (PaginationInterceptor)**: 集成了强大的物理分页插件，配置简单。
        - **其他功能**: 如主键生成策略、逻辑删除、乐观锁、性能分析插件等。
    - **建议**: 在掌握了 MyBatis 基础之后，**强烈建议学习和使用 MyBatis Plus**，它能显著提高开发效率，是目前国内 Java 后端非常流行的实践。
5. **理解 MyBatis 插件 (Interceptor)**: 允许你在 MyBatis 执行 SQL 的关键节点（如参数处理、SQL 准备、结果集处理）进行拦截和修改，可以实现自定义功能，如分页、分表、数据权限、审计日志等。分页插件就是基于此原理。
6. **缓存策略**: 深入理解 MyBatis 缓存机制，并学习如何整合分布式缓存（如 Redis）来提升应用性能。
7. **整合数据库中间件**: 如 ShardingSphere (原 Sharding-JDBC)，用于实现数据库的读写分离、分库分表，MyBatis 可以与其良好集成。

**总结与建议**

1. **从 Spring Boot 集成开始**: 这是最贴近实际应用、最高效的学习路径。
2. **动手实践**: 跟着教程或文档，自己动手写代码、配置、运行，遇到问题解决问题。
3. **理解核心原理**: 不要满足于能跑通，要理解 Mapper 接口、XML、`SqlSession`、参数映射、结果映射等核心概念。
4. **掌握动态 SQL**: 这是发挥 MyBatis 灵活性的关键。
5. **进阶学习 MyBatis Plus**: 掌握基础后，务必学习 MyBatis Plus，它将极大提升你的开发效率。

#### mybatis.mapper-locations属性配置
- **`classpath:`**: 这个前缀是 Spring 的标准用法，意思是“从 classpath 路径下开始查找”。在一个标准的 Maven 或 Gradle 项目中，`src/main/resources/` 目录下的所有内容在运行时都会被自动添加到 classpath 中。所以 `classpath:` 就代表了 `src/main/resources/` 这个目录。
- **`/`**: 路径分隔符。
- **`*` (星号)**: 通配符，匹配路径中的 0 个或多个字符。例如，`mapper/*.xml` 会匹配 `mapper/UserMapper.xml`，但不会匹配 `mapper/user/UserMapper.xml`。
- **`**` (双星号)**: 通配符，匹配 0 个或多个目录层级。例如，`mapper/**/*.xml` 会匹配 `mapper/UserMapper.xml`，`mapper/user/UserMapper.xml`，`mapper/order/detail/OrderMapper.xml` 等等。**这是最常用、最灵活的配置方式**，因为它允许你将 XML 文件组织在 `mapper` 目录下的任意子目录中。
- **情况一 (最推荐)：** 所有的 Mapper XML 文件都放在 `src/main/resources/mapper/` 目录下，**允许有子目录**。
    ```
    # application.properties
    mybatis.mapper-locations=classpath:mapper/**/*.xml
    ```

- **情况二：** 所有的 Mapper XML 文件都**直接**放在 `src/main/resources/mapper/` 目录下，**没有子目录**。
    ```PropertiesProperties
    # application.properties
    mybatis.mapper-locations=classpath:mapper/*.xml
    ```
    
### 项目结构
![[Pasted image 20250413212004.png]]

### 下一步学习
1. **动态SQL**
	- 定义一个方法，接受一个包含所有可能条件的参数对象（或者 Map）。在 XML 中只写一个对应 `id` 的 SQL 标签，但在标签内部使用 MyBatis 的动态 SQL 功能（如 `<if>`, `<choose>`, `<when>`, `<otherwise>`, `<where>`, `<set>` 等）来根据传入参数的值动态地构建不同的 SQL 语句。
2. 配置

