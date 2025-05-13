## Servlet

* 项目分为**工作空间项目**和**tomcat部署的项目**，这两者是一个项目但是存储的位置不一样，**tomcat部署的项目存储在out目录下**；
  
  * tomcat访问的是tomcat部署的项目，而tomcat部署的项目对应着工作空间的项目
  
  * WEB-INF目录下的资源不能被浏览器直接访问

* urlpartten：servlet访问路径
  * 一个servlet可以定义多个访问路径：{"/d1","d2"}等
  * 可以定义多层路径 ： /xxx/xxx
  * 通配符 
    * */xxx
    * xxx/*
    * *.do

### http协议

* 概念：超文本传输协议，定义了客户端和服务器端通信时，发送**数据的格式**

* 特点：

  1. 基于**TCP/IP**的高级协议

  2. **默认端口：80**

  3. 基于请求/响应模型：一次请求对因一次响应

  4. **无状态的**：每次请求之间相互独立，不能交互数据据

  5. *一个页面中所有的图片资源、css资源、js文件资源都是单独的请求，既一个页面的显示需要多次请求*，network（网络资源）能展示请求的资源

     

#### 请求数据的格式

1. **请求行**

   * 请求方式	请求url	请求协议/版本
   * 例：GET      /login.html    HTTP/1.1

2. **请求头**

   * 请求头是客户端与服务器之间的 “沟通协议”

   * *和请求行没有空行*

3. 请求空行

   * 空行（作用：分隔请求体）

4. **请求体**（正文）

   * 键值对，用=赋值，用&连接
   * 例 ：` username=Java009&password=123456`
   
   

* 请求标头 （字符串格式）

![image-20250318122734655](D:\Jyl\笔记\图片\image-20250318122734655.png)



##### 请求方法

*请求方法有7种，常用的有2种*

* **GET：**
  1.  请求参数在请求行中，在url中的 ？ 后
  2. 请求的url长度有限制
  3. 不太安全
* **POST：**
  1. 请求参数**在请求体中** 
  2. 请求的url长度没有限制
  3. 相对安全



##### 常见的请求头

1. **User-Agent**：浏览器的版本信息
   * 获取信息后可以解决浏览器的兼容性问题
2. **Referer：** *告诉服务器，我从哪里来*
   * **作用**：
     1. 放盗链
     2. 统计工作
3. Accept (支持) ：支持的文件格式
4. **Connection：**连接状态

### Request

 ##### request对象和response对象的原理
   
   * request对象和response对象**由服务器创建**，我们来使用
   * request对象用来**获取请求消息**，response对象用来**设置响应消息**
   
##### request功能

   1. **获取请求消息数据**
      * 获取请求行 -- `String getMethod()`
      * 获取虚拟目录 -- `String getContextpath()`
      * 获取servlet路径 -- `String getServletPath()`
      * 获取get方式的请求参数 -- `String getQueryString()`
      * 获取协议及版本 -- `String getProtocol()`
      * 获取请求url 
		- `String getRequestURI() : /xx/xxx`
		- `StringBuffer getRequestURL() : http://xx/xxx`
   
   2. 获取请求头数据

   3. 获取请求体数据
	- *步骤*
	     1. **获取流对象**
	        - `BufferedReader  gerReader()` : 获取字符输入流，只能操作字符数据
	        * `ServletInputStream  getInpueStream()` : 获取字节输入流，操作所有类型数据
		2. 从流中获取数据
		      - `String readLine()` : 获取一行字符串

#####  其他功能

   ###### **获取请求参数的通用方法**

   * `String getParameter(String name)` ： 根据传入的参数名称**获取参数值**

   * `String[] getPatameterValues(String name)` ： 根据传入的参数名获取参数值的数组

   * `Enumeration<String> getParameterNames()` ： **获取所有请求的参数名称**

   * `Map<String,String[]> getParameterMap()` ： 获取所有参数的Map集合

###### **中文乱码问题**

- tomcat 8以后已经解决了get请求方法的乱码问题
-  *post请求方法：设置流的编码* -- `request.setCharacterEncoding("UTF-8");`

   ###### 请求转发

   * **一种在服务器内部的资源跳转方式**

   * *步骤*
	1. 通过request对象获取转发器对象：
	    `RequestDispatcher getRequestDispatcher(String path)`
	 2.  使用RequestDispatcher对象来进行转发：
	   	`forward(ServlerRequest request,ServletResponse response)`
* *特点*
     1. 浏览器地址栏路径不发生变化
     2. 只能转发到当前服务器内部资源中
     3. *转发是一次请求*
   
   ###### 共享数据 
   
   * 域对象：一个有作用范围的对象，可以**在范围内共享数据**
   * *request域对象*：代表一次请求的范围，一般用于请求转发的多个资源中去共享数据
   * 方法：
     * `void serAttribute(String name,Object obj)`  存储数据
     * `Object getAttribute(String name)`  通过键获取值
     * `void removeAttribute(String name)`  通过键值对移除数据
   
   ###### 获取ServletContext
   * `ServletContext getServletContext()`  获取ServletContext

### Cookie

* 概念

  简单地说，**Cookie 是服务器在与客户端通信时使用的存储在客户端的一小段数据**。

  **它们用于**在发送后续请求时识别客户端。它们还可用于将一些数据从一个 servlet 传递到另一个 servlet。

* 创建Cookie
  * 实例化Cookie对象 -- `Cookie uiColorCookie = new Cookie("color", "red");`
  * 将Cookie添加到响应中 -- `response.addCookie(uiColorCookie);`

* 常用方法

  * **设置Cookie有效期**

    `void setMaxAge()` 

    -- d单位为s，超过此期限，客户端发送请求时无法使用该cookie，该cookie从浏览器缓存中删除

  * **设置Cookie的域**

    `setDomain(String s)`

    -- 这允许我们指定客户端应将其传送到的域名。这还取决于我们是否明确指定域名。

    -- 如果我们没有明确指定域，它将被设置为 创建 cookie 的域名。

  * **设置Cookie路径**

    `setPath(String path)`

    -- 该路径指定了 cookie 将被传送到的位置。

    -- 如果我们明确指定路径，则Cookie将被传递到给定的 URL 及其所有子目录

    -- 隐式地，它将被设置为创建 cookie 的 URL 及其所有子目录。

  * **在Servlet中读取Cookies**

    `cookies[] getCookies()`

    -- Cookies 由客户端添加到请求中。客户端检查其参数并决定是否可以将其传递到当前 URL。

  * **删除Cookie**

    `Cookie userNameCookieRemove = new Cookie("userName", "");
    userNameCookieRemove.setMaxAge(0);
    response.addCookie(userNameCookieRemove);`

    -- 要从浏览器中删除 cookie，我们必须在响应中添加一个同名的新 cookie，但将maxAge值设置为0

### HttpSession

* **概念**

  -- HttpSession是跨不同请求存储用户相关数据的另一种选择。会话是保存上下文数据的服务器端存储。

  -- 不同会话对象之间不共享数据（客户端只能从其会话访问数据）。它也包含键值对，但与 cookie 相比，会话可以包含对象作为值。存储实现机制依赖于服务器。

* **获取对话**

  `request.getSession(boolean flag)`

  -- true为创建新的会话，默认值为true 

  -- false获取现有会话，也可以在配置中设置false来禁止默认创建会话

  * 在大多数情况下，Web 服务器**使用 Cookie 进行会话管理**。创建会话对象时，服务器会创建一个**带有*JSESSIONID*键和值的 Cookie，用于标识会话。**

* **会话属性**

  会话对象提供了一系列方法来访问（创建、读取、修改、删除）为给定用户会话创建的属性：

  - `setAttribute(String, Object)` -- 使用键和新值创建或替换会话属性
  - `getAttribute(String)` -- 读取具有给定名称（键）的属性值
  - `removeAttribute(String)` -- 删除具有给定名称的属性
  - `getAttributeNames()` -- 检查已存在的会话属性
  - `session.invalidate();`哦 -- 从 Web 服务器中删除整个会话

### Fileter

* 概念

  Fileter是Servlet2.3版本引入的一种新的组件。Filter过滤器是一个对象，它可以在请求到达Servlet之前对其进行预处理，也可以在相应离开Servlet后对其进行后处理。过滤器能动态的拦截请求和响应，并对其包含的信息进行转换或使用。

  与Servlet不同的是，过滤器通常不直接生成响应，而是提供一种通用的功能，可以“附加”到任何类型的Servlet或JSP页面上。

* 应用场景

  * 对访问资源的请求进行身份验证和授权
  * 记录请求和响应的日志信息，进行审计
  * 修改响应的头部和数据，例如压缩响应数据、添加Cookie、对响应数据进行加密
  * 进行令牌化处理
  * 触发资源访问事件

* 注解

  `@WebFilter("/*")`

  -- 一般用通配符

* **放行** 

  对指定页面放行，**根据需求逻辑确定**

  *以下是拦截未登录用户访问资源*

  1. 登录、注册等页面

  2. 静态资源 (html、css、js等文件)

  3. 指定操作 (不用登录也可以执行的操作)

  4. 登录状态 (判断session中用户信息是否为空)

* 跳转到指定界面

  `response.sendRedirect("Login.jsp");`

