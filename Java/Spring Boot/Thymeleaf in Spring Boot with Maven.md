

### Thymeleaf Initialization

1. 在`pom.xml`中先加入thymeleaf

```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-thymeleaf</artifactId>  
</dependency>
```

2. 設定要返回view的controller

+ 回傳String時，只需要回傳檔名就好，不需要extension
```java
package com.example.demo;  
  
import org.springframework.stereotype.Controller;  
import org.springframework.ui.Model;  
import org.springframework.web.bind.annotation.GetMapping;  
  
@Controller  
public class IndexController {  
  
    @GetMapping("/")  
    public String index(Model model){  
        model.addAttribute("message", "Hello! My server!");  
        return "index";  
    }  
}
```

>[!info]
> 因為這部分是要透過thymeleaf幫我們render template， 所以需要回傳`view`，必須使用`@Controller` ，因為如果使用`@RestController`的話，會直接被作為Response body以`json/xml`的格式回傳


### Thymeleaf Basic Syntax

```html
<!DOCTYPE html>  
<html xmlns:th="https://www.thymeleaf.org" lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
</head>  
<body>  
    <h1 th:text="${message}"></h1>  
</body>  
</html>
```

+ 通常在HTML tag中以`th:`開頭，表示要使用thymeleaf的功能

### Reference

[[Day17] Thymeleaf 輕鬆入門 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10301576)

