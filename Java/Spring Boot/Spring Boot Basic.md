

### Overview

+ Fullstack framework

### Initialization

1. `Spring Initializr`官方選擇好後，將檔案下載下來
2. 透過intellij內建的`spring initializr`，基本上跟網頁版的會是一樣的流程

#### Dependency Choice

+ web
	+ spring web
+ SQL
	+ Spring Data JPA
+ Apache Derby Database
	+ postgres

## 常見的annotation

### @Controller

+ 通常用於建立傳統的的web application controller
+ 回傳值通常會是`view`的名稱，用於render

### @RequestBody

+ 用來接收RequestBody中的資料，但通常是接受`appliction/json`的Content-Type
	+ 如果使用前端表單預設的method進行傳輸的話，預設的Content-Type是`application/x-www-form-urlencoded`，所以要透過`@RequestBody`進行拿取時，需要更改前端的 Content-Type。

### @ResponseBody

+ 可以回傳object，會將object內部欄位或是key/value pair轉換成`json`
+ 回傳string，將純文字放在`response body`中

想要對response有更高的掌控權可以透過`ResponseEntity`來達成

+ entity中的generic可以`指的是response body中的型態`

```java
@RestController
public class MyController {

    @GetMapping("/custom")
    public ResponseEntity<String> getCustom() {
        HttpHeaders headers = new HttpHeaders();
        headers.add("Custom-Header", "value");
        return new ResponseEntity<>("Hello, Custom World", headers, HttpStatus.CREATED);
    }
}

```


### @RestController

+ `@Controller的特化版` 
	+ 將`@Controller`以及`@ResponseBody`合在一起
	+ 簡化開發，且會將return的內容自行轉換成`response body`，通常為==JSON或XML格式==
	+ 過去要回傳response body的時候，還需要加上`@Response body`

```java

//with RestController
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyRestController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }
}

```

```java
//without reponse body
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class MyController {

    @GetMapping("/hello")
    @ResponseBody
    public String hello() {
        return "Hello, World!";
    }
}

```
### @RequestMapping

+ 類似於Express中的router
+ 透過指定特定path value，來設定`api endpoint`
+ 還有許多更特定`HTTP Method`的做法
	+ GetMapping
	+ PostMapping
	+ PutMapping
	+ DeleteMapping


## Dependency Injection(DI)

+ 指的是dependency被spring boot自動注入需要依賴的一方
+ 在需要依賴的地方加上`@Autowired` annotation，通常會加在class的constructor上


以下方程式碼來說，StudentService是StudentController的dependency，而DI指會在StudentController收到request的時候，自動去幫我們建立StudentService，在這邊也就是dependency的角色，所以才會被稱作dependency injection。
```java
@RestController  
@RequestMapping(path="api/v1/student")  
public class StudentController {  
  
    private final StudentService studentService;  
  
    @Autowired  
    public StudentController(StudentService studentService){  
        this.studentService = studentService;  
    }  
    @GetMapping()  
    public List<Student> getStudents() {  
        return studentService.getStudents();  
    }  
}
```

### Beans

+ 與前面提到的DI有關，只要是被需要的dependency通常都應該要加上相關的annotation註冊在spring boot中，讓spring boot在需要他時去幫我們進行DI
+ 以下是常見的四個類型
	+ `Component`
		+ 可以用於任何位置
	+ `Service`
		+ semantic的component，通常會用在`service layer`
	+ `Repository`
		+ semantic的component, 通常會用在`model`，用來與db溝通
	+ `Confiugrtion`
		 `@Configuration`用來修飾class，代表是一個配置類，通常包含許多`@bean`或annotation， `@Configuration`代表的是`@Bean`定義的來源。	
	+ `Bean`
		+ 通常會搭配`@Configuration` annotation，用來作為配置類。
		
		+ 用於method level，其中回傳值會被加入IoC Container中，讓後續的其他部分使用 
		
		+ Springboot允許透過參數來進行`Autowire`，這也是為甚麼常常在配置檔案中參數看似沒有傳遞卻也能正常運作的原因。
		
		+ 以下面的程式碼來說，redisConnectionFactory就是一個被自動注入的參數

		+ ==Bean 的名稱就是該 method 的名稱==
		
```java
package com.example.demo.config;  
  
import com.example.demo.service.RedisService;  
import lombok.extern.log4j.Log4j2;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.data.redis.connection.RedisConnectionFactory;  
import org.springframework.data.redis.core.RedisTemplate;  
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;  
import org.springframework.data.redis.serializer.StringRedisSerializer;  
  
@Log4j2  
@Configuration  
public class RedisConfig {  
    @Bean  
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {  
       RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();  
        try {  
            redisTemplate.setKeySerializer(new StringRedisSerializer());  
            redisTemplate.setValueSerializer(new StringRedisSerializer());  
            redisTemplate.setHashKeySerializer(new StringRedisSerializer());  
            redisTemplate.setHashValueSerializer(new GenericJackson2JsonRedisSerializer());  
            redisTemplate.setConnectionFactory(redisConnectionFactory);  
        } catch (Exception e) {  
            log.error("Error getting Redis Template connection", e);  
        }  
        return redisTemplate;  
    }  
}
```


### @Component vs @Configuration + @Bean

| 比較項目          | `@Component`（含 `@Service` 等）     | `@Bean` + `@Configuration`    |
| ------------- | -------------------------------- | ----------------------------- |
| 用法            | 標註在 class 上                      | 寫在 `@Configuration` 類別的方法中    |
| 掃描註冊方式        | 被 Spring 掃描（透過 `@ComponentScan`） | 透過 Java 程式碼手動建立               |
| 適合的場合         | 自己寫的類別，邏輯清晰、有實體類別                | 第三方 lib（你不能標註它們）或需自定建構流程      |
| 控制靈活性         | 相對簡單                             | 更靈活，可控制建構邏輯、參數傳入、條件等          |
| 是否為 Singleton | 預設為 Singleton（除非設成 prototype）    | 預設為 Singleton（除非手動改）          |
| 常見場景          | 服務、DAO、Controller 等              | 需要細節初始化的外部類別、factory、client 等 |

```
Spring Application 啟動
        │
        ▼
───────────────
| Component Scan | ← 掃描 @Component, @Service, @Repository, @Controller
───────────────
        │
        ▼
   建立 Bean
 (由 Spring 自動 new)
        │
        ▼
IoC 容器註冊 Bean (id = 類名小寫)

───────────────
| Configuration | ← 找到所有 @Configuration 標註的類
───────────────
        │
        ▼
執行內部所有 @Bean 標註的方法
        │
        ▼
方法 return 出來的物件被註冊為 Bean
        │
        ▼
IoC 容器註冊 Bean (id = 方法名)
```

### DI with Inteface

有時我們會透過implement interface，來確保功能沒有被漏掉(`因為interface的method需要全都被實作`)。舉例來說`UserService`及`UserServiceImpl`，在進行DI的時候==注入interface比較好==，因為這樣其他地方的程式碼只會依賴於interface。

且在進行DI時，==如果interface只有一個implementation==，spring就會直接替我們去找這個implementation。

如果是有多個implementation的狀況，則需要再額外使用`Qualifier`進行標記

## Access Parameter in URL

+ Path Variable
```java
@GetMapping("/users/{userId}/orders/{orderId}")
public String getOrder(@PathVariable String userId, @PathVariable String orderId, Model model) {
    model.addAttribute("userId", userId);
    model.addAttribute("orderId", orderId);
    return "orderDetails";
}

```

+ Query Parameter
```java
@GetMapping("/search")
public String search(@RequestParam String query, @RequestParam int page, Model model) {
    model.addAttribute("query", query);
    model.addAttribute("page", page);
    return "searchResults";
}
```

## Redirect

### @Model

+ 在進行view的render時所使用的數據容器
+ 可以透過`Model`或`ModelMap`將資料傳遞給template engine
### @ModelAttribute

+ 用於將請求參數綁定到model中，讓參數可以被更方便的取用

假設有一個表單資料包含使用者email及password，我們將其封裝在一個叫做`User`的class。我們就可以搭配`@ModelAttribute`來取得資料

```java
@Controller
public class MyController {

    @RequestMapping("/form")
    public String handleForm(@ModelAttribute("user") User user) {
        // 处理用户输入
        return "result";
    }
}

```

### @ModelAttribute vs @RequestParam

這兩種方法都會去擷取攜帶在URL中的參數，以下是各自的使用場景

+ ModelAttribute  
	+ 需要將多參數綁訂到一個對象上時，像是上面的`User`例子
	+ 需要在多個Controller method間`共享數據時`
+ RequestParam
	+ 只是要從請求中獲得單個獲少量參數時

### RedirectView vs ModelAndView

+ RedirectView
	+ 專門用於redirect
	+ 直接返回redirect過去的View，且會一併改變URL
	+ 適合純粹的redirect需求
```java
@GetMapping("/redirect1")
public RedirectView redirect1() {
    RedirectView redirectView = new RedirectView();
    redirectView.setUrl("/target-url");
    
    // 添加參數
    redirectView.addFlashedAttribute("param1", "value1");
    redirectView.addFlashedAttribute("param2", "value2");
    
    return redirectView;
}
```
+ ModelAndView
	+ 用於返回View和Model的資料
	+ 可以用於render template或redirect
	+ 較為靈活，`可以同時傳遞model資料以及redirect`
```java
@GetMapping("/redirect")
public ModelAndView redirect() {
    ModelAndView modelAndView = new ModelAndView();
    //添加模型參數
    modelAndView.addObject("key", "value"); // 添加數據到model
    modelAndView.setViewName("redirect:/target-url"); // 設置重導向的目標URL
    return modelAndView;
}
```

## redirectattribute.addAttribute vs redirectattribute.addFlashAttribute

+ `addAttibute` - 將重導向的網址加入到URL中，可以透過`request param`拿取
+ `addFlashAttibute` - 透過建立一個一次性的session，來在下一次的重導向中使用資料
	+ 可以透過`modelattribute`拿取
```java
@GetMapping("/signup")  
public String getSignup(@ModelAttribute("message") String message){  
    return "signup";  
}  
  
/**  
 * * @param userDto - user info provided by client  
 * @param redirectAttributes - used to pass attribute when redirecting.  
 * @return ModelView  
 */@PostMapping("/signup")  
public RedirectView postSignup(UserDto userDto, RedirectAttributes redirectAttributes){  
    AuthResult result = userService.signup(userDto);  
    redirectAttributes.addFlashAttribute("message", result.getMessage());  
    return new RedirectView("/"+result.getFilePath());  
}
```

## Tomcat

+ 開源的Java Servelet container以及webserver

+ Spring Boot預設使用的servelet container，也可以自行選擇`undertow`、`jetty`做使用，但內建的tomcat效能可能會較差一點，所以在要放到production環境時，有時會放在獨立的tomcat中

+ muti-thread，適合運算密集性的task

## Servelet

+ 是一個Java class，用於擴展服務器的功能，處理客戶端請求並生成動態Web內容。

+ 可以處理各類型的請求，但通常用於HTTP請求

+ 可以動態生成web page

+ 支持session manage

>[!info]
> 一個Servlet 本質上是一個特殊的Java類，`設計用來處理Web請求並生成響應。它是Java Web應用程序中處理HTTP請求的基本單位。每個Servlet都有自己的職責，通常負責特定類型的請求或特定功能區域的處理。在整個Web應用中可能有多個Servlet，每個都處理不同的URL或不同類型的任務`
>
> 可以暫時將一個Servelet想像成一個controller的感覺，這些不同的Servelet如何處理請求就是Servelet container的責任


## How Servelet work with Servelet Container

1. Container接收到HTTP request
2. 創建ServeletRequest及ServeletResponse
3. 選擇合適的Servelet來處理請求
4. 調用Servelet的`service` method
5. 回傳response給client