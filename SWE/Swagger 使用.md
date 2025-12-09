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


### 統一錯誤處理

如果每個 API 都需要使用 `@ApiResponses` 去註解的話，可能會讓程式碼變的很冗長。在這樣的狀況下，可以統一將常見的 response 寫成 annotation，讓共用性的 response 可以被其他 controller 使用

```java
package com.example.demo.config;  
  
import com.example.demo.dto.ErrorResponse;  
import com.example.demo.dto.ErrorResponseDto;  
import io.swagger.v3.oas.annotations.media.Content;  
import io.swagger.v3.oas.annotations.media.Schema;  
import io.swagger.v3.oas.annotations.responses.ApiResponse;  
import io.swagger.v3.oas.annotations.responses.ApiResponses;  
  
import java.lang.annotation.ElementType;  
import java.lang.annotation.Retention;  
import java.lang.annotation.RetentionPolicy;  
import java.lang.annotation.Target;  
  
/**  
 * Swagger 錯誤回應的可重用註解  
 *  
 * 使用方式：  
 * 在 Controller 方法上直接使用這些組合註解，減少重複的 @ApiResponses 宣告  
 *  
 * 範例：  
 * @Operation(summary = "根據 ID 取得使用者")  
 * @StandardGetResponses  
 * @GetMapping("/{id}")  
 * public ResponseEntity<UserDto> getUserById(@PathVariable Long id) {  
 *     // ... * } */public class SwaggerResponseConfig {  
  
    /**  
     * CRUD 查詢操作的標準回應（包含 200, 401, 404, 500）  
     */  
    @Target({ElementType.METHOD})  
    @Retention(RetentionPolicy.RUNTIME)  
    @ApiResponses(value = {  
            @ApiResponse(  
                    responseCode = "200",  
                    description = "操作成功"  
            ),  
            @ApiResponse(  
                    responseCode = "401",  
                    description = "未經授權 - JWT Token 無效或過期",  
                    content = @Content(schema = @Schema(implementation = ErrorResponse.class))  
            ),  
            @ApiResponse(  
                    responseCode = "404",  
                    description = "找不到指定的資源",  
                    content = @Content(schema = @Schema(implementation = ErrorResponseDto.class))  
            ),  
            @ApiResponse(  
                    responseCode = "500",  
                    description = "伺服器內部錯誤",  
                    content = @Content(schema = @Schema(implementation = ErrorResponse.class))  
            )  
    })  
    public @interface StandardGetResponses {  
    }  
  
    /**  
     * CRUD 建立操作的標準回應（包含 200, 400, 401, 409, 500）  
     */  
    @Target({ElementType.METHOD})  
    @Retention(RetentionPolicy.RUNTIME)  
    @ApiResponses(value = {  
            @ApiResponse(  
                    responseCode = "200",  
                    description = "建立成功"  
            ),  
            @ApiResponse(  
                    responseCode = "400",  
                    description = "請求參數驗證失敗或格式錯誤",  
                    content = @Content(schema = @Schema(implementation = ErrorResponseDto.class))  
            ),  
            @ApiResponse(  
                    responseCode = "401",  
                    description = "未經授權 - JWT Token 無效或過期",  
                    content = @Content(schema = @Schema(implementation = ErrorResponse.class))  
            ),  
            @ApiResponse(  
                    responseCode = "409",  
                    description = "資料衝突 - 資料重複或存在關聯限制",  
                    content = @Content(schema = @Schema(implementation = ErrorResponseDto.class))  
            ),  
            @ApiResponse(  
                    responseCode = "500",  
                    description = "伺服器內部錯誤",  
                    content = @Content(schema = @Schema(implementation = ErrorResponse.class))  
            )  
    })  
    public @interface StandardCreateResponses {  
    }  
  
    /**  
     * CRUD 更新操作的標準回應（包含 200, 400, 401, 404, 409, 500）  
     */  
    @Target({ElementType.METHOD})  
    @Retention(RetentionPolicy.RUNTIME)  
    @ApiResponses(value = {  
            @ApiResponse(  
                    responseCode = "200",  
                    description = "更新成功"  
            ),  
            @ApiResponse(  
                    responseCode = "400",  
                    description = "請求參數驗證失敗或格式錯誤",  
                    content = @Content(schema = @Schema(implementation = ErrorResponseDto.class))  
            ),  
            @ApiResponse(  
                    responseCode = "401",  
                    description = "未經授權 - JWT Token 無效或過期",  
                    content = @Content(schema = @Schema(implementation = ErrorResponse.class))  
            ),  
            @ApiResponse(  
                    responseCode = "404",  
                    description = "找不到指定的資源",  
                    content = @Content(schema = @Schema(implementation = ErrorResponseDto.class))  
            ),  
            @ApiResponse(  
                    responseCode = "409",  
                    description = "資料衝突 - 資料重複或存在關聯限制",  
                    content = @Content(schema = @Schema(implementation = ErrorResponseDto.class))  
            ),  
            @ApiResponse(  
                    responseCode = "500",  
                    description = "伺服器內部錯誤",  
                    content = @Content(schema = @Schema(implementation = ErrorResponse.class))  
            )  
    })  
    public @interface StandardUpdateResponses {  
    }  
  
    /**  
     * CRUD 刪除操作的標準回應（包含 204, 401, 404, 409, 500）  
     */  
    @Target({ElementType.METHOD})  
    @Retention(RetentionPolicy.RUNTIME)  
    @ApiResponses(value = {  
            @ApiResponse(  
                    responseCode = "204",  
                    description = "刪除成功"  
            ),  
            @ApiResponse(  
                    responseCode = "401",  
                    description = "未經授權 - JWT Token 無效或過期",  
                    content = @Content(schema = @Schema(implementation = ErrorResponse.class))  
            ),  
            @ApiResponse(  
                    responseCode = "404",  
                    description = "找不到指定的資源",  
                    content = @Content(schema = @Schema(implementation = ErrorResponseDto.class))  
            ),  
            @ApiResponse(  
                    responseCode = "409",  
                    description = "資料衝突 - 無法刪除，存在關聯資料",  
                    content = @Content(schema = @Schema(implementation = ErrorResponseDto.class))  
            ),  
            @ApiResponse(  
                    responseCode = "500",  
                    description = "伺服器內部錯誤",  
                    content = @Content(schema = @Schema(implementation = ErrorResponse.class))  
            )  
    })  
    public @interface StandardDeleteResponses {  
    }  
  
    /**  
     * 通用錯誤回應（401, 500）- 適用於所有需要認證的 API  
     */    @Target({ElementType.METHOD})  
    @Retention(RetentionPolicy.RUNTIME)  
    @ApiResponses(value = {  
            @ApiResponse(  
                    responseCode = "401",  
                    description = "未經授權 - JWT Token 無效或過期",  
                    content = @Content(schema = @Schema(implementation = ErrorResponse.class))  
            ),  
            @ApiResponse(  
                    responseCode = "500",  
                    description = "伺服器內部錯誤",  
                    content = @Content(schema = @Schema(implementation = ErrorResponse.class))  
            )  
    })  
    public @interface CommonErrorResponses {  
    }  
  
    /**  
     * Admin 專用操作的回應（包含 200, 401, 403, 404, 500）  
     * 適用於需要 ADMIN 權限的操作  
     */  
    @Target({ElementType.METHOD})  
    @Retention(RetentionPolicy.RUNTIME)  
    @ApiResponses(value = {  
            @ApiResponse(  
                    responseCode = "200",  
                    description = "操作成功"  
            ),  
            @ApiResponse(  
                    responseCode = "401",  
                    description = "未經授權 - JWT Token 無效或過期",  
                    content = @Content(schema = @Schema(implementation = ErrorResponse.class))  
            ),  
            @ApiResponse(  
                    responseCode = "403",  
                    description = "權限不足 - 需要 ADMIN 權限",  
                    content = @Content(schema = @Schema(implementation = ErrorResponse.class))  
            ),  
            @ApiResponse(  
                    responseCode = "404",  
                    description = "找不到指定的資源",  
                    content = @Content(schema = @Schema(implementation = ErrorResponseDto.class))  
            ),  
            @ApiResponse(  
                    responseCode = "500",  
                    description = "伺服器內部錯誤",  
                    content = @Content(schema = @Schema(implementation = ErrorResponse.class))  
            )  
    })  
    public @interface AdminOperationResponses {  
    }  
}
```

在 controller 中使用
```java
@Operation(  
        summary = "取得當前使用者資訊",  
        description = "取得當前登入使用者的詳細資訊。需要 JWT 認證。"  
)  
@SecurityRequirement(name = "bearer-jwt")  // ← 這行會顯示 🔒@StandardGetResponses  
@GetMapping("/me")  
public ResponseEntity<UserDto> getCurrentUser() {  
    // 實作：從 JWT token 取得當前使用者  
    return ResponseEntity.ok(new UserDto());  
}  
  
@Operation(  
        summary = "取得所有使用者",  
        description = "取得系統中所有使用者列表。需要 JWT 認證。"  
)  
@SecurityRequirement(name = "bearer-jwt")  // ← 這行會顯示 🔒@StandardGetResponses  
@GetMapping("/users")  
public ResponseEntity<List<UserDto>> getAllUsers() {  
    // 實作：取得所有使用者  
    return ResponseEntity.ok(List.of());  
}
```

## Reference

- [iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/m/articles/10362925)

