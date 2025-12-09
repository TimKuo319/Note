---
tags:
  - is/evergreen/substantiated
---

## 前置設定

在 spring boot 3.x.x 版本以後，需使用 `springdoc-openapi-starter-webmvc-ui` 來顯示 swagger 

### pom.xml

```xml
<dependency>  
    <groupId>org.springdoc</groupId>  
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>  
    <version>2.7.0</version>  
</dependency>
```

>[!info]
>在使用時需要注意 `springdoc-openapi-starter-webmvc-ui` 與 spring boot 的版本相容性，以我目前使用的版本來說，3.4.1 的 spring boot 搭配上 2.6.0 版本的 `springdoc-openapi-starter-webmvc-ui` 就會因為某個 function 不支援，要搭配 2.7.0 版本的 `springdoc-openapi-starter-webmvc-ui` 才可以

### application.properties
```yml
springdoc.api-docs.path=/v3/api-docs  
springdoc.swagger-ui.path=/swagger-ui.html  
springdoc.swagger-ui.enabled=true  
springdoc.swagger-ui.tryItOutEnabled=true
```

### 配置 Configuration

```java
@Configuration  
public class OpenApiConfig {  
  
    @Bean  
    public OpenAPI customOpenAPI() {  
        return new OpenAPI()  
				// swagger 基本資訊說明	
                .info(new Info()  
                        .title("Demo API")  
                        .version("1.0")  
                        .description("Spring Boot REST API with JWT Authentication")  
                        .contact(new Contact()  
                                .name("API Support")  
                                .email("support@example.com"))  
                        .license(new License()  
                                .name("Apache 2.0")  
                                .url("https://www.apache.org/licenses/LICENSE-2.0.html")))  
				// 添加驗證方式
                .components(new Components()  
                        .addSecuritySchemes("bearer-jwt", new SecurityScheme()  
                                .type(SecurityScheme.Type.HTTP)  
                                .scheme("bearer")  
                                .bearerFormat("JWT")  
                                .description("JWT token authentication")))  
				// 將驗證方式套用到所有 API 
                .addSecurityItem(new SecurityRequirement().addList("bearer-jwt"));  
    }  
} 
```

## Usage

## Reference

- [iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/m/articles/10362925)

