---
tags:
  - Java
  - SpringBoot
---
## What is `Object mapper`, why we need it

在現今傳遞訊息的格式中，`json` 已經算是非常主流的傳輸格式。而 `object mapper` 則是用來協助我們進行這些格式之間的轉換。
## Usage

假設我們有一個 User class

```Java
public class User{
	String username;
	Integer age;
}
```

### writeValueAsString()

writeValueAsString 的方式會將傳入的物件轉換成 json 字串。

```java
import com.fasterxml.jackson.databind.ObjectMapper;

public class Example {
    public static void main(String[] args) throws Exception {
        ObjectMapper objectMapper = new ObjectMapper();

        // 建立一個示例物件
        User user = new User("Alice", 30);

        // 將物件轉換為 JSON 字符串
        String jsonString = objectMapper.writeValueAsString(user);
        System.out.println(jsonString);  // 輸出 JSON
    }
}
```

### readValue

readValue 會將 json 字串轉換為目標格式。

```java
String jsonString = "{\"name\":\"Alice\", \"age\":30}";
User user = objectMapper.readValue(jsonString, User.class);
System.out.println(user.getName());  // 輸出：Alice
```

也可以轉換為 List 或 Map 這樣的資料格式

```java
String jsonString = "{\"name\":\"Alice\", \"age\":30}";
Map<String, Object> map = objectMapper.readValue(jsonString, new TypeReference<Map<String, Object>>(){});
System.out.println(map.get("name"));  // 輸出：Alice
```

## Reference

[SpringBoot - 使用 ObjectMapper 完成 json 和 Java Object 互相轉換](https://kucw.io/blog/2020/6/java-jackson/)