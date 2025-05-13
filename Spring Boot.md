# Spring Boot

#### 注解

* **`@Data`**
  实体类，自动补全get和set方法
* `@NoArgsConstructor`
  无参构造
* `@AllargsConsructor`
  全参构造
* **`@RestController`:**
  - *用途:*  主要用于**构建 RESTful API**，这类 API 通常直接返回数据 (例如 JSON 或 XML) 给客户端，而不是 HTML 页面。
  - 特点:
    - 是 `@Controller` 和 `@ResponseBody` 注解的组合。
    - `@ResponseBody` 注解表示该控制器方法的返回值应该直接作为 HTTP 响应的 body 发送给客户端，而不是被解析为视图名。
    - 方法的返回值通常是 Java 对象，Spring MVC 会**自动将其转换为 JSON 或 XML 等格式 (通过 `HttpMessageConverter`)。
  - *适用场景:*  构建**前后端分离**的应用，或者为移动应用、其他服务提供数据接口。
*  `@RequestMapping`
	- 路径
- **`@RequestParam`**
	- **用途：** 放在 Controller 方法的参数上。
	- **属性：**
	    - `value` 或 `name`: 指定请求参数名。
	    - `required`: boolean 类型，默认 `true`，表示必须存在对应名字的值，否则报错。
	    - `defaultValue`: 设置参数缺失时的默认值。
	- 作用： 将请求 URL 中带 `?key=value` 形式的参数绑定到方法参数上。**
#### 声明bean的注解
1. @Component -- 基础声明
2. @Controller -- 标注在控制层类上
3. @Service -- 标注在业务层类上
4. @Repository -- 标注在数据访问层类上(由于与mybatis整合，用的少)
### IOC和DI
- 候补 -- JavaWeb 48集

#### bean类的命名
*  如果在声明时没有命名，则bean的类名位首字母小写的类名
*  注解上可以直接命名
#### @ComponentScan扫描
* 前面声明bean的四大注解，**必须要被组件扫描注解@ComponentScan扫描才能生效**
* 该注解包含在了启动类声明注解@SpringBootApplication中，默认扫描的范围是**启动类所在的包及其子包**
### DI详解
#### 依赖注入方式
* 基于@Autowired进行依赖注入的常见方式有以下三种
	1. 属性注入
		* 声明属性，在属性上添加@Autowired
		* **缺点：** *隐藏了类之间的依赖关系、可能回破坏类的封装性*
	2. 构造函数注入
		* **用final声明属性**，再写一个带参的构造函数，**在构造函数中为属性赋值**，构造函数上面添加@Autowired。
		* 如果当前类中只存在一个构造函数，那么@Autowired可以省略
		* **优点：** 能清晰地看到类的依赖关系、提高了代码的安全性
		* **缺点：** 代码繁琐、，如果构造参数过多，可能回到是构造函数臃肿
	3. setter注入
		- 基于set方法进行注入，在set方法上添加@Autowired 
		- **优点：** 保持了类的封装性，依赖关系清晰
		- **缺点：** 需要额外编写setter方法，增加了代码量
> [!node] **使用推荐**
> - 追求**简洁性**使用**属性注入**
> -  追求**规范性**使用**构造函数注入**

#### DI多个bean注入错误
- @Autowired注解，默认是按照**类型**注入的
- 如果存在**多个相同类型的bean**，则会报错
- 解决方案：
	1. @Primary 
		在bean上添加此注解，表示该bean优先注入
	2. @Qualifier
		配合@Autowired使用，在该注解中配置bean的名字，表示注入该名字的bean
	3. @Resource
		不需要在写@autowired，给该注解配置name值，表示注入该名字的bean
		- 与@Autowired的区别
			1. @Autowired由Spring框架提供，@Resource由Java EE规范提供
			2. @Autowired默认按照类型注入，@Resource默认按照名称注入
### AOP 面向切面编程
Aspect-Oriented Programming
- **核心思想：**
	1. **识别“切面”（Aspect）**：将这些**通用的、分散的功能**（如日志、安全、事务）定义为“切面”。
	2.  **定义“切点”（Pointcut）**：**明确这些“切面”应该在哪些类的哪些方法上生效**（比如，“所有 `com.example.service` 包下的 `save*` 方法”）。
	3. **定义“通知”（Advice）**：规定“切面”里的代码具体在“切点”方法执行的**什么时机运行**（比如，在方法执行前 `@Before`，方法执行后 `@After`，方法正常返回后 `@AfterReturning`，抛出异常后 `@AfterThrowing`，或者环绕整个方法执行 `@Around`）。
	4. **织入（Weaving）**：通过某种机制（在 Spring 中通常是运行时通过动态代理），**将“切面”代码动态地应用到目标“切点”上**，而不需要修改目标方法的源代码。
- **Boot中使用：**
	1. 添加Srarter依赖 -- Aspect-Oriented Programming
	2. 注解驱动 -- 使用`@Aspect`, `@Pointcut`, `@Before`, `@After`, `@Around` 等注解来定义切面、切点和通知
### JDBC

##### MySQL驱动
- [MySQL驱动安装网址]([MySQL :: Download MySQL Connector/J (Archived Versions)](https://downloads.mysql.com/archives/c-j/))
- pom配置
``` xml
<dependency>  
    <groupId>mysql</groupId>  
    <artifactId>mysql-connector-java</artifactId>  
    <version>8.0.33</version>  
</dependency>
```

#### 1. 使用方式
1. 注册驱动
	- `Class.forName("com.mysql.cj.jdbc.Driver");`
2. 获取连接对象
	- `Connection connection = DriverManager.getConnection(url, username, password);`
	   - **url格式 --** `jdbc:mysql://hostname:port/databaseName`
		   - hostname -- MySQL地址
		   - port -- 连接端口
		   - databaseName -- 具体的数据库名称
		   - `localhost:3306`可以省略 -- `jdbc:mysql:///databaseName`
3. 获取预编译SQL语句对象
	- `PreparedStatement preparedStatement = connection.prepareStatement(String sql);`
4. 为占位符赋值并执行,接受返回的结果
	- `preparedStatement.setString(1,String str);` -- 1代表的是占位符的索引，从1开始
	   `ResultSet resultSet = preparedStatement.executeQuery();`
5. 处理结果: 遍历resultSet结果集
6. 释放资源 -- 先开后关原则
##### 1.1 注册驱动
>[!node]
>在Java中，当使用JDBC（JavaDatabaseConnectivity）连接数据库时，需要加载数据库特定的驱动程序，以便与数据库进行通信。加载驱动程序的目的是为了注册驱动程序，使得JDBCAPI能够识别并与特定的数据库进行交互。
> 从JDK6开始，不再需要显式地调用Class.forName()来加载JDBC驱动程序，只要在类路径中集成了对应的jar文件，会自动在初始化时注册驱动程序。
##### 1.2 Connection
- **Connection接口是JDBCAPI的重要接口,用于建立与数据库的通信通道。** 换而言之,Connection对象不为空,则代表一次数据库连接。
- 在建立连接时,需要指定数据库URL、用户名、密码参数。**对连接进行配置：在URL后面添加？后传递参数。**
- **Connection接口还负责管理事务,Connection接口提供了commit和rollback方法,用于提交事务和回滚事务。**
- 可以创建Statement对象,用于执行SQL语句并与数据库进行交互。
- **在使用JDBC技术时,必须要先获取Connection对象,在使用完毕后,要释放资源,避免资源占用浪费及泄漏。**

##### ~~Statement~~
- **Statement接口用于执行SQL语句并与数据库进行交互。** 它是JDBC API中的一个重要接口。通过Statement对象,可以向数据库发送SQL语句并获取执行结果。
- 结果可以是一个或多个结果。
	- 增删改：受影响行数单个结果。
	- 查询：单行单列、多行多列、单行多列等结果。
- Statement接口在执行SQL语句时,会产生==**SQL注入攻击问题**==
	- 当使用Statement执行动态构建的SQL查询时，往往需要将查询条件与SQL语句拼接在一起，直接将参数和SQL语句一并生成，让SQL的查询条件始终为true得到结果。
##### 1.3 PreparedStatement
- **PreparedStatement是Statement接口的子接口，用于执行预编译的SQL查询**
	- 预编译SQL语句：在创建PreparedStatement时，就会预编译SQL语句，也就是SQL语句已经固定。
- **防止SQL注入：**
	- PreparedStatement支持参数化查询，将数据作为参数传递到SQL语句中，采用？占位符的方式，将传入的参数用一对单引号包裹起来“”，并将参数中的关键符号前加上转移符号，使关键符号无效化，有效防止传入关键字或值导致SQL注入问题。
- 性能提升：
	- PreparedStatement是预编译SQL语句，同一SQL语句多次执行的情况下，可以复用，不必每次重新编译和解析。
##### 1.4 ResuleSet
- ResultSet是JDBCAPI中的一个接口，用于表示从数据库中**执行==查询语句==所返回的结果集**。它提供了一种用于遍历和访问查询结果的方式。
- 遍历结果：ResultSet可以使用next()方法将游标移动到结果集的下一行，逐行遍历数据库查询的结果，返回值为boolean类型，true代表 有下一行结果，false则代表没有。
- 获取单列结果：可以通过*getXXX（XXX为数据类型）* 方法获取单列的数据，该方法为重载方法，支持传递索引或列名参数进行获取，推荐使用类型获取。
---
#### ORM思想
Object Relational Mapping
- **概念：对象到关系数据库的映射**，作用是在编程中，把面向对象的概念跟数据库中表的概念对应起来，以面向对象的角度操作数据库中的数据，即一张表对应一个类，一行数据对应一个对象，一个列对应一个属性！
- 当下JDBC中这种过程我们称其为手动ORM。后续会学习ORM框架，比如MyBatis、JPA等。

###### 实体类
- 在使用JDBC操作数据库时，我们会发现数据都是零散的，明明在数据库中是一行完整的数据，到了Java中变成了一个一个的变量，不利于维护和管理。而我们Java是面向对象的，一个表对应的是一个类，一行数据就对应的是Java中的一个对象，一个列对应的是对象的属性，所以我们要把数据存储在一个载体里，这个载体就是实体类！

###### 主键回显
- 在数据中,执行新增操作时,主键列为自动增长,可以在表中直观的看到,但是在Java程序中,我们执行完新增后,只能得到受影响行数,无法得知当前新增数据的主键值。
  **在Java程序中获取数据库中插入新数据后的主键值,并赋值给Java对象,此操作为主键回显。**
- 代码实现
	- `PreparedStatement preparedStatement = connection.prepareStatement(String sql, Statement.RETURN_GENERATED_KEYS);`
	- 在预编译对象的构造方法中sql语句后面添加一个值，这个值可以从Statement的成员变量中获取
	- `ResultSet generatedKeys = preparedStatement.getGeneratedKeys();`
	- 获取预编译对象执行后回显的主键
	- 主键回显的结果集在方法外定义，在结束时关闭资源

###### 批量操作
- 概念/逻辑：
	- 本质上是一条sql语句传递多个值的语法
	- **可以极大的加快多条数据写入的速度**
- 注意：
	1. **在连接数添加?rewriteBatchedStatements=true参数，表示允许批量操作**
	2. 新增SQL必须用values，且语句最后不要追加 ; 结束 
	3. 赋值时调用`addBatch()`方法，将SQL语句进行批量添加操作，也就是重新赋值一组
	4. 统一执行批量操作，调用`executeBatch()`方法

---
#### 连接池
- **概念：**
	- **连接池就是数据库连接对象的缓冲区，通过配置，由连接池负责创建连接、管理连接、释放连接等操作。**
- **注意**
	- 预先创建数据库连接放入连接池，用户在请求时,通过池直接获取连接，使用完毕后，将连接放回池中，避免了频繁的创建和销毁，同时解决了创建的效率。
	- 当池中无连接可用，且未达到上限时，连接池会新建连接。
	- 池中连接达到上限,用户请求会等待,可以设置超时时间。
- **使用：**
	- JDBC的数据库连接池使用==**javax.sql.DataSource==接口**进行规范,所有的第三方连接池都实现此接口,自行添加具体实现！
	  也就是说，**所有连接池获取连接的和回收连接方法都一样，不同的只有性能和扩展功能！**
- 常用连接池：
	- DBCP是Apache提供的数据库连接池,速度相对C3P0较快,但自身存在一些BUG
	- C3P0 是一个开源组织提供的一个数据库连接池,速度相对较慢,稳定性还可以。
	- Proxool 是sourceforge下的一个开源项目数据库连接池，有监控连接池状态的功能,稳定性较c3po差一点
	- ==**Druid**== 是阿里提供的数据库连接池，是集DBCP、C3PO、Proxool 优点于一身的数据库连接池，性能、扩展性、易用性都更好，功能丰富。
	- ==**Hikari**==*（ひかり[shi ga li]）取自日语，是光的意思*，是SpringBoot2.x之后内置的一款连接池，基于 BoneCP (已经放弃维护,推荐该连接池)做了不少的改进和优化,口号是快速、简单、可靠。  
> 追求扩展性选择Druid（德鲁伊）
> 追求极致的速度选择Hikari（系咖喱）

##### Druid
- 硬编码
	1. 创建DruidDataSource连接池对象  
	2. 设置连接池的配置信息【必须 | 非必须】  
	3. 通过连接池获取连接对象  
	4. 回收连接【非释放连接，而是将连接归还给连接池】
- 软编码（常用，解耦性）
	1. 编写配置文件
	2. 创建Properties集合，读取外部配置文件输入流并加载到Properties集合
	3. 使用DruidDataSourceFactory基于Peoperties中的配置构建连接池
		- **软编码实际上是应用程序在运行时读取外部配置文件，并根据这些配置信息在代码内部动态地创建和实例化连接池对象，从而实现连接池参数的配置。**
	4. 通过连接池获取连接
	5. 开发CRUD
	6. 回收连接
##### Hikari
- 基本和Druid连接池实现步骤相同，不同点有以下：
	1. 初始连接 -- `setMinimumIdle()`
	2. 最大连接 -- `setMaximumPoolSize()`
	3. 设置url -- `setJdbcUrl()`
	4. **不通过DruidDataSourceFactory配置连接池，而是将Propertiesc传入HikariConfig，用HikariConfig配置连接池**

### JDBC优化及工具类封装
#### ThreadLocal
> *JDK 1.2的版本中就提供java.lang.ThreadLocal，为解决多线程程序的并发问题提供了一种新的思路。使用这个工具类可以很简洁地编写出优美的多线程程序。通常用来在在多线程中管理共享数据库连接、Session等。*
- 简介
	- ThreadLocal用于保存某个线程共享变量，原因是在Java中，每一个线程对象中都有一个ThreadLocalMap<ThreadLocal, Object>，其key就是一个ThreadLocal，而Object即为该线程的共享变量。  
	- 而这个map是通过ThreadLocal的set和get方法操作的。对于同一个static ThreadLocal，不同线程只能从中get，set，remove自己的变量，而不会影响其他线程的变量。
- *应用场景*
	1. **在进行对象跨层传递的时候，使用ThreadLocal可以避免多次传递，打破层次间的约束。**
	2. **线程间数据隔离。**
	3. 进行事务操作，用于存储线程事务信息。
	4. 数据库连接，Session 会话管理。
- 常用方法
	1. ThreadLocal对象.get(): 获取ThreadLocal中当前线程共享变量的值。
	2. ThreadLocal对象.set(): 设置ThreadLocal中当前线程共享变量的值。
	3. ThreadLocal对象.remove(): 移除ThreadLocal中当前线程共享变量的值。  
---
> [!注意事项]
1. **必须清理 (`remove()`):** 再次强调，由于 Web 服务器使用线程池，线程会被复用。如果不调用 `remove()`，上一个请求设置的数据会“泄漏”到下一个使用相同线程的请求中，导致数据错乱和潜在的安全问题，同时也会造成内存泄漏（`ThreadLocalMap` 中的 Entry 无法被回收）。
2. **`InheritableThreadLocal`:** 这是 `ThreadLocal` 的一个子类，允许子线程继承父线程的值。在 Web 应用中要**谨慎使用**，因为你通常不希望请求处理线程中创建的子线程（例如异步任务 `@Async`）继承请求特定的数据（如用户信息），除非你明确知道需要这样做，并且正确地管理这些子线程中的数据和清理。对于标准请求处理流程，普通 `ThreadLocal` 通常是正确的选择。
3. **静态持有者:** 使用 `static final ThreadLocal` 是常见模式，确保 `ThreadLocal` 对象本身是单例的，但其内部存储的值是每个线程独立的。
---
#### Dao
*DAO: Data Access Object, 数据访问对象。*
- **Dao层**
	- Java是面向对象语言，数据在Java中通常以对象的形式存在。一张表对应一个实体类，一张表的操作对应一个DAO对象！
	  在Java操作数据库时，我们会将对同一张表的增删改查操作统一维护起来，维护的这个类就是DAO层。
	  DAO层只关注对数据库的操作，供业务层Service调用，将职责划分清楚！
- **BaseDAO概念**  
	- 基本上每一个数据表都应该有一个对应的DAO接口及其实现类，发现对所有表的操作（增、删、改、查）代码重复度很高，所以可以抽取公共代码，给这些DAO的实现类可以抽取一个公共的父类，复用增删改查的基本操作，我们称为BaseDAO。  


