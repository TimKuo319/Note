

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

### @ResponseBody

+ Tells a controller that the object returned is automatically serialized into JSON and passed back into the _HttpResponse_ object.
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
	+ `Bean`

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