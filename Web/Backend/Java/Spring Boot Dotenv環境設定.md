
## 安裝dotenv套件

### Maven

+ 將以下套件加入pox.xml中
```xml
<dependency>  
    <groupId>io.github.cdimascio</groupId>  
    <artifactId>dotenv-java</artifactId>  
    <version>3.0.0</version>  
</dependency>
```

## 新增.env至根目錄

```.env
DB_URL=...
DB_USERNAME=...
DB_PASSWORD=...
```

## 配置Configuration class

可以選擇將configuratoin class配置到與main同一個package下(`不用手動指定componentscan路徑的作法`)

```java
package org.example.assignment3.config;  
  
import io.github.cdimascio.dotenv.Dotenv;  
import jakarta.annotation.PostConstruct;  
import org.springframework.context.annotation.Configuration;  
  
@Configuration  
public class DotenvConfig {  
  
    @PostConstruct  
    public void loadEnv(){  
        Dotenv dotenv = Dotenv.configure().load();  
        dotenv.entries().forEach(entry -> {  
            System.setProperty(entry.getKey(),entry.getValue());  
        });  
    }  
}
```

## @Configuration與@PostConstruct

+ Configuration
	+ 一個比較特別的component，相對於其他component通常會在要被`injection`時才進行初始化，`@Configuration`標記的class會在最一開始進行instantiate，來讓配置能被最開始載入。
+ PostConstruct
	+ 

