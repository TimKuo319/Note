
## 安裝套件

```pom.xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-aop</artifactId>  
</dependency>
```

## 啟用AOP

```java
package org.example.stylish;  
  
import org.springframework.boot.SpringApplication;  
import org.springframework.boot.autoconfigure.SpringBootApplication;  
import org.springframework.context.annotation.EnableAspectJAutoProxy;  
  
@SpringBootApplication  
@EnableAspectJAutoProxy //加上註解 
public class StylishApplication {  
  
    public static void main(String[] args) {  
        SpringApplication.run(StylishApplication.class, args);  
    }  
  
}
```


