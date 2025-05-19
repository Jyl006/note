## 理论

### **什么是第三方API调用？**

简单来说，就是你的应用程序（后端或前端）通过网络向另一个服务提供者（第三方）发送请求，获取数据或触发某个操作。这个“请求”和“响应”遵循一种约定的格式和规则，这个约定就是API（应用程序接口）。

### **为什么需要调用第三方API？**

- **获取外部数据：** 例如天气预报、地图信息、汇率、股票数据等。
- **利用外部服务：** 例如发送短信/邮件、支付、文件存储、机器翻译、AI能力等。
- **集成其他系统：** 例如登录第三方账号（微信、支付宝）、调用电商平台的API进行商品管理等。

### **核心原理：HTTP请求**

调用第三方API的核心通常是通过发送 **HTTP请求** 并接收 **HTTP响应** 来实现的。你需要了解以下几个关键概念：

1. **API端点 (Endpoint)：** 这是一个URL，指定了你要访问的API的具体位置和功能。例如 `https://api.example.com/users/123`。
2. **HTTP方法 (HTTP Method)：** 指明你想要对资源执行的操作。
    - `GET`: 获取数据 (最常用)。
    - `POST`: 提交数据，创建新资源 (常用)。
    - `PUT`: 更新资源。
    - `DELETE`: 删除资源。
    - 其他方法如 PATCH, HEAD, OPTIONS 等。
3. **请求头 (Headers)：** 提供了关于请求的元数据，例如内容类型 (`Content-Type`)、认证信息 (`Authorization`)、用户代理 (`User-Agent`) 等。
4. **请求体 (Body)：** 在 `POST` 或 `PUT` 请求中，用于发送需要提交的数据，通常是JSON或表单数据。
5. **HTTP状态码 (Status Code)：** 响应中包含的三位数字，表示请求处理的结果。
    - `2xx` (例如 200 OK): 请求成功。
    - `4xx` (例如 400 Bad Request, 401 Unauthorized, 404 Not Found): 客户端错误。
    - `5xx` (例如 500 Internal Server Error): 服务器端错误。
6. **响应头 (Response Headers)：** 提供了关于响应的元数据。
7. **响应体 (Response Body)：** API返回的数据，通常是JSON格式。

### **关键考虑因素：**

1. **认证与授权 (Authentication & Authorization)：** 大多数第三方API为了安全和计费，要求你在请求中提供凭证，证明你有权限调用。常见的认证方式有：
    - **API Key：** 在请求头或URL参数中附带一个密钥。
    - **OAuth 2.0：** 一种授权框架，通过令牌 (Token) 来证明授权。
    - **基本认证 (Basic Auth)：** 使用用户名和密码编码后放在请求头。 你需要仔细阅读API文档，了解它采用哪种认证方式以及如何正确提供凭证。
2. **数据格式：** 绝大多数现代API使用 **JSON (JavaScript Object Notation)** 作为数据交换格式。你需要学习如何在你的编程语言中解析（从JSON字符串转为对象）和构建（从对象转为JSON字符串）JSON数据。
3. **错误处理：** API调用可能会失败，原因多样（网络问题、认证失败、参数错误、服务器内部错误等）。你需要根据HTTP状态码和响应体中的错误信息来判断失败原因，并进行相应的处理（例如重试、给用户提示）。
4. **异步处理：** API调用通常需要网络传输时间，是阻塞操作。在后端，为了不阻塞主线程，通常使用异步客户端或在单独的线程中执行。在前端（JavaScript），API调用是天然异步的，使用Promise或async/await来处理结果。

### **在你的技术栈中如何实践：**

#### **1. 后端 (Java / Spring Boot)**

在Java中进行HTTP请求，你可以使用多种库：

- **`java.net.HttpURLConnection`：** Java自带的库，比较底层，使用稍显繁琐。
- **Apache HttpClient：** 一个功能强大且广泛使用的第三方库。
- **Spring WebClient：** Spring WebFlux 提供的响应式 HTTP 客户端，也非常适合在 Spring Boot 中使用，推荐学习和使用。

**使用 Spring WebClient 的大致步骤：**

1. **添加依赖：** 如果使用 WebClient，确保你的 Spring Boot 项目有 `spring-boot-starter-webflux` 或 `spring-boot-starter-web` (后者也可以用WebClient，但Webflux更匹配其响应式特性) 的依赖。
2. **创建 WebClient 实例：** 可以配置 base URL, headers 等。

    ```Java
    WebClient webClient = WebClient.builder()
        .baseUrl("https://api.example.com") // 第三方API的基础URL
        //.defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE) // 默认请求头
        //.defaultHeader("Authorization", "Bearer your_token") // 认证信息
        .build();
    ```
    
3. **构建并发送请求：**

    ```Java
    // GET 请求示例
    SomeResponse response = webClient.get()
        .uri("/data/{id}", 123) // API的具体路径，可以带参数
        //.header("Custom-Header", "value") // 添加特定请求头
        .retrieve() // 发送请求并获取响应
        .bodyToMono(SomeResponse.class) // 将响应体解析为指定的Java对象 (需要Jackson或Gson)
        .block(); // 阻塞等待结果 (在WebFlux应用中通常不用block，而是返回Mono/Flux)
    
    // POST 请求示例
    SomeRequestBody requestBody = new SomeRequestBody("some data");
    AnotherResponse postResponse = webClient.post()
        .uri("/resource")
        .bodyValue(requestBody) // 设置请求体，Spring Boot会自动序列化为JSON
        .retrieve()
        .bodyToMono(AnotherResponse.class)
        .block();
    ```
    
    这里的 `SomeResponse`, `SomeRequestBody`, `AnotherResponse` 是你需要根据第三方API返回的JSON结构定义相应的Java类（POJO），通常需要用到 Jackson 或 Gson 这样的JSON处理库来辅助解析。
4. **处理响应：** 检查状态码，处理解析后的数据或错误。
5. **配置管理：** API Key 或其他敏感信息应该放在 Spring Boot 的配置文件 (`application.properties` 或 `application.yml`) 中，并通过 `@Value` 或 `@ConfigurationProperties` 读取，避免硬编码。

#### **2. 前端 (Vue3)**

在前端（浏览器环境）进行HTTP请求：

- **`Workspace` API：** 浏览器原生提供的API，基于Promise，是现代前端推荐使用的。
- **Axios：** 一个非常流行的基于Promise的HTTP客户端库，功能更强大，使用更方便，支持请求拦截、响应拦截等。推荐前端使用Axios。

**使用 Axios 的大致步骤：**

1. **安装 Axios：** `npm install axios` 或 `yarn add axios`
    
2. **导入 Axios：** `import axios from 'axios';`
    
3. **发送请求：** Axios 返回 Promise，可以使用 `.then().catch()` 或 `async/await` 来处理。

    ```JavaScript
    // GET 请求示例 (在 Vue 组件的方法或 setup 中)
    async fetchData() {
      try {
        const response = await axios.get('https://api.example.com/data/123', {
          headers: { // 请求头
            'Authorization': 'Bearer your_token'
          },
          params: { // URL查询参数
            query: 'test'
          }
        });
        console.log('Data:', response.data); // response.data 就是解析后的JSON数据
        // 更新 Vue 组件的状态
      } catch (error) {
        console.error('Error fetching data:', error);
        // 处理错误，例如显示错误消息给用户
      }
    }
    
    // POST 请求示例
    async postData() {
      try {
        const postBody = { name: 'New Item', value: 10 };
        const response = await axios.post('https://api.example.com/resource', postBody, {
          headers: {
            'Content-Type': 'application/json'
          }
        });
        console.log('Post Response:', response.data);
      } catch (error) {
        console.error('Error posting data:', error);
      }
    }
    ```
    
4. **处理响应：** `response.data` 通常就是第三方API返回的JSON数据对应的JavaScript对象。
    
5. **处理跨域问题 (CORS)：** 浏览器有同源策略，如果你的前端应用和第三方API不在同一个域名下，可能会遇到跨域问题。通常需要在后端（你的Spring Boot应用作为代理，或者第三方API本身）进行CORS配置，允许你的前端域名访问。或者在开发环境中配置 Vue CLI 或 Vite 的代理。
    
### 本质
**不同软件系统之间，按照约定好的规则（API规范），通过发送请求和接收响应来进行数据交换或功能协作。**

更深入一点理解：

1. **它是一种通信：** 就像人与人之间说话交流一样，API是软件系统之间“说话”的方式。你的程序向第三方服务“说”出它的需求（发送请求），第三方服务处理后，“说”出结果（返回响应）。
2. **它基于契约（合同）：** 第三方服务通过API文档定义了它能提供的功能（例如：能查天气、能发短信）、如何调用这些功能（用什么网址、什么方法GET/POST、需要提供什么参数、放在请求的哪里）以及返回的结果是什么格式（通常是JSON）。你的程序必须严格遵守这个“契约”来构造请求，第三方服务也必须按照契约来处理请求并返回结果。
3. **它实现了功能复用和解耦：** 调用API的根本目的是为了利用别人已经提供的能力，而不用自己从头开发。这让你的系统可以专注于自己的核心业务，而将其他专业功能（如支付、地图、人工智能）交给更专业的第三方服务去完成，降低了系统的复杂度和耦合度。

所以，本质上，调用第三方API就是让你的程序成为一个“客户端”，向提供服务的“服务器”发出符合约定的“指令”（请求），然后接收并处理“回馈”（响应），从而实现特定目标。这是一个**基于契约的网络通信过程**。

### **学习建议：**

1. **找一个简单的公共API练手：** 例如：
    - [Public APIs](https://github.com/public-apis/public-apis) 这个仓库收集了很多免费开放的API。
    - 一些天气API、笑话API等。
2. **仔细阅读API文档：** 这是最重要的。文档会告诉你API的端点、支持哪些HTTP方法、需要哪些参数、如何认证、返回的数据格式是什么样的以及可能的错误码。
3. **先从简单的GET请求开始：** 获取一些公开的数据，不涉及认证。
4. **然后尝试需要认证的API：** 学习如何在请求头或参数中加入API Key 或 Token。
5. **练习发送POST请求：** 学习如何构建请求体发送数据。
6. **学习JSON解析：** 确保你能将API返回的JSON数据正确地转换为你的程序可以使用的对象。在Java用Jackson/Gson，在JavaScript中原生支持JSON解析（Axios/fetch会自动解析）。
7. **实践错误处理：** 模拟或故意制造一些错误情况（例如，不传必需参数，使用错误的认证信息），看看API返回什么，并学习如何优雅地处理这些错误。
8. **在Spring Boot中封装API调用：** 将第三方API的调用逻辑封装在一个Service类中，提供清晰的方法供其他部分调用。
9. **在Vue3中封装API调用：** 可以在一个单独的模块中创建函数来调用API，或者使用组合式函数封装，然后在组件中调用这些函数。

## 实践过程

### 第一次实现API调用

```Java
package cn.moonify.weatherapicall.service;  
  
import org.json.JSONArray;  
import org.json.JSONObject;  
  
import java.io.BufferedReader;  
import java.io.InputStreamReader;  
import java.net.HttpURLConnection;  
import java.net.URI;  
import java.net.URL;  
  
public class WeatherService {  
    private static final String API_URL = "https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/";  
    private static final String API_KEY = "your_api_key"; // 替换为你的 API 密钥  
  
    public String getWeather(String city) throws Exception {  
        String urlString;// = API_URL + city + "?key=" + API_KEY;  
        // URL硬编码示例  
        urlString = "https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&hourly=temperature_2m";  
        URL url = URI.create(urlString).toURL(); // 推荐用法，兼容新旧版本  
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();  
        connection.setRequestMethod("GET");  
  
        int responseCode = connection.getResponseCode();  
        if (responseCode == 200) {  
            BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()));  
            String inputLine;  
            StringBuilder response = new StringBuilder();  
            while ((inputLine = in.readLine()) != null) {  
                response.append(inputLine);  
            }  
            in.close();  
            // 测试：System.out.println(response.toString());  
            return parseWeatherResponse(response.toString());  
        } else {  
            throw new Exception("Failed to fetch weather data: " + responseCode);  
        }  
    }  
  
    private String parseWeatherResponse(String response) {  
        JSONObject json = new JSONObject(response);  
        String timezone = json.getString("timezone");  
        String temperatureUnits = json.getJSONObject("hourly_units").getString("temperature_2m");  
        JSONArray hourlyTime = json.getJSONObject("hourly").getJSONArray("time");  
        JSONArray hourlyTemperature = json.getJSONObject("hourly").getJSONArray("temperature_2m");  
        StringBuilder weatherInfo = new StringBuilder();  
        weatherInfo.append("Timezone: ").append(timezone).append("\n");  
        weatherInfo.append("Hourly Temperature:\n");  
        for (int i = 0; i < 24; i++) {  
            String time = hourlyTime.getString(i);  
            double temperature = hourlyTemperature.getDouble(i);  
            weatherInfo.append(String.format("%s: %.2f %s\n", time, temperature, temperatureUnits));  
        }  
        return weatherInfo.toString();  
  
    }  
}
```

#### 收获

1. 寻找第三方API，并查阅API文档
2. 建立url，将连接转为HttpURLConnection对象
3. 发送请求，如果成功则通过IO流接受响应数据
4. 解析响应数据（一般为JSON文件）
5. 下一步学习Spring WebClient


