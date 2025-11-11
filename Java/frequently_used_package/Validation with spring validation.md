
## 引入套件

```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-validation</artifactId>  
</dependency>
```

## 使用

在class member上進行宣告
```java
package com.example.demo.DTO;  
  
import jakarta.validation.constraints.NotEmpty;  
  
public class userDTO {  
  
    @NotEmpty  
    private String username;  
    @NotEmpty  
    private String password;  
  
    public userDTO(String username, String password){  
        setUsername(username);  
        setPassword(password);  
    }  
  
    public String getUsername() {  
        return username;  
    }  
  
    public void setUsername(String username) {  
        this.username = username;  
    }  
  
    public String getPassword() {  
        return password;  
    }  
  
    public void setPassword(String password) {  
        this.password = password;  
    }  
}
```

在要驗證的參數上加上`@Valid`

```java
@Validated  
@Controller  
public class IndexController {  
  
    @GetMapping("/")  
    public String Hello(Model model) {  
       model.addAttribute("message", "Hello! Welcome to My Server!");  
       return "index";  
    }  
  
    @PostMapping("/login")  
    public void Login(@Valid userDTO userDto, Model model) {  
        System.out.println(userDto);  
    }  
}
```

## Exception Handler

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import java.util.HashMap;
import java.util.Map;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, String>> handleValidationExceptions(
            MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(error -> 
            errors.put(error.getField(), error.getDefaultMessage())
        );
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(errors);
    }
}
```


## @Validated vs @Valid

+ `@Validated 的作用`：
	- 當應用於參數時，它指定了該參數應該使用哪個驗證組。
	- 當應用於class時，它為該類的所有方法參數啟用驗證。
	+ 分組驗證
```java
// 定義驗證組
public interface CreateGroup {}
public interface UpdateGroup {}

public class User {
    @NotBlank(groups = {CreateGroup.class, UpdateGroup.class})
    private String name;

    @NotBlank(groups = CreateGroup.class)
    @Email(groups = {CreateGroup.class, UpdateGroup.class})
    private String email;

    @NotNull(groups = CreateGroup.class)
    @Min(value = 18, groups = {CreateGroup.class, UpdateGroup.class})
    private Integer age;

    // 只在更新時需要
    @NotNull(groups = UpdateGroup.class)
    private Long id;

    // getters and setters
}
```

在controller中使用
```java
@RestController
@Validated
public class UserController {
    @PostMapping("/users")
    public ResponseEntity<String> createUser(@Validated(CreateGroup.class) @RequestBody User user) {
        // 創建用戶邏輯
        return ResponseEntity.ok("User created");
    }

    @PutMapping("/users/{id}")
    public ResponseEntity<String> updateUser(@Validated(UpdateGroup.class) @RequestBody User user) {
        // 更新用戶邏輯
        return ResponseEntity.ok("User updated");
    }
}
```

其中的分組通常會是空的interface

```java
public interface CreateGroup {}
public interface UpdateGroup {}
```


## Reference

[Day 16 - Spring Boot 資料驗證 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10275699)
