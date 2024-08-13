
## Configuration

```xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.34</version>
    <scope>provided</scope>
</dependency>
```

## Usage

在要使用的class上加上`@Data` annotation

```java
package com.example.demo.DTO;  
  
import jakarta.validation.constraints.NotBlank;  
import lombok.Data;  
  
@Data  
public class userDTO {  
  
    @NotBlank(message = "invalid username")  
    private String username;  
    @NotBlank(message = "invalid password")  
    private String password;  
  
    public userDTO(){  
  
    }  
  
    public userDTO(String username, String password){  
        setUsername(username);  
        setPassword(password);  
    }  
}
```


## Constructor annotation

+ `@NoArgsConstructor`
	+ 生成空參數建構子
+ `@RequriedArgsConstructor`
	+ 生成包含必須member的contructor
	+ ex: 對於有`final` keyword或`@NotNull`的member 
```java
@RequiredArgsConstructor
public class Example {
    private final String name;
    @NonNull
    private Integer age;
    // 自动生成包含必需字段的构造函数
}

```
+ `@AllArgsConstructor`
	+ 生成所有memeber組合的constructor

+ `@Data`，以下四種annotation的組合
	+ `@Getter/@Setter`
	+ `@ToString`
	+ `@RequiredArgsConstructor`
	+ `@EqualsAndHashCode`
## Reference

[用 Lombok 讓程式碼更簡潔. 開場先來個簡介 | by Program Walker | Medium](https://medium.com/@program.walker1997/%E7%94%A8-lombok-%E8%AE%93%E7%A8%8B%E5%BC%8F%E7%A2%BC%E6%9B%B4%E7%B0%A1%E6%BD%94-8f5f1bc23d3e)
