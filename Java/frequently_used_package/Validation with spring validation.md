# Spring Validation 完整指南

## 一、基本介紹

Spring Validation 是基於 Java Bean Validation (JSR-303/JSR-380) 規範的數據驗證機制，用於驗證請求參數、請求體物件等數據的合法性。

### 添加依賴
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

## 二、常用驗證註解

### 2.1 空值檢查

- `@NotNull` - 值不能為 null
- `@NotEmpty` - 值不能為 null 且長度/大小必須大於 0（適用於 String、Collection、Map、Array）
- `@NotBlank` - 值不能為 null 且去除空白後長度必須大於 0（僅適用於 String）

**使用建議:**
- String 類型使用 `@NotBlank`
- Collection、Map、Array 使用 `@NotEmpty`
- 其他類型使用 `@NotNull`

### 2.2 數值相關

- `@Min(value)` - 數值必須大於等於指定值
- `@Max(value)` - 數值必須小於等於指定值
- `@DecimalMin(value)` - 數值必須大於等於指定值（支援小數）
- `@DecimalMax(value)` - 數值必須小於等於指定值（支援小數）
- `@Positive` - 數值必須為正數
- `@PositiveOrZero` - 數值必須為正數或零
- `@Negative` - 數值必須為負數
- `@NegativeOrZero` - 數值必須為負數或零
- `@Digits(integer, fraction)` - 數值的整數部分和小數部分位數限制

### 2.3 字串相關

- `@Size(min, max)` - 字串、集合、陣列的大小必須在指定範圍內
- `@Pattern(regexp)` - 字串必須符合正則表達式
- `@Email` - 字串必須是有效的電子郵件格式

### 2.4 日期時間相關

- `@Past` - 日期必須是過去的時間
- `@PastOrPresent` - 日期必須是過去或現在
- `@Future` - 日期必須是未來的時間
- `@FutureOrPresent` - 日期必須是未來或現在

### 2.5 其他

- `@AssertTrue` - 值必須為 true
- `@AssertFalse` - 值必須為 false

## 三、@Valid vs @Validated 差異比較

### 3.1 核心差異

| 特性                  | @Valid                    | @Validated       |
| ------------------- | ------------------------- | ---------------- |
| **來源**              | Jakarta Validation (標準規範) | Spring Framework |
| **使用位置**            | 方法參數、欄位                   | 類級別、方法參數         |
| **支援分組驗證**          | ❌ 不支援                     | ✅ 支援             |
| **支援嵌套驗證**          | ✅ 支援                      | ✅ 支援             |
| **驗證 @RequestBody** | ✅ 可用                      | ✅ 可用             |
| **驗證方法參數**          | ❌ 不可用                     | ✅ 可用（需類上加註解）     |

### 3.2 @Valid 使用場景

#### 場景 1: 驗證請求體物件 (@RequestBody)
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @PostMapping
    public ResponseEntity<?> createUser(@Valid @RequestBody UserDTO userDTO) {
        // @Valid 驗證 userDTO 的所有欄位
        return ResponseEntity.ok("創建成功");
    }
}
```

#### 場景 2: 嵌套物件驗證
```java
public class UserDTO {
    @NotBlank
    private String name;
    
    @Valid  // 重要! 嵌套物件需要加 @Valid
    private AddressDTO address;
    
    @Valid
    private List<PhoneDTO> phones;  // 集合中的物件也需要驗證
}

public class AddressDTO {
    @NotBlank
    private String city;
    
    @NotBlank
    private String street;
}
```

**重點:** `@Valid` 可以遞迴驗證嵌套物件，但嵌套物件的欄位上也必須加 `@Valid`。

### 3.3 @Validated 使用場景

#### 場景 1: 驗證方法參數 (@PathVariable, @RequestParam)
```java
@RestController
@RequestMapping("/api/users")
@Validated  // 必須在類上加 @Validated!
public class UserController {
    
    @GetMapping("/{id}")
    public ResponseEntity<?> getUser(
            @PathVariable 
            @Min(value = 1, message = "ID 必須大於 0") 
            Long id) {
        return ResponseEntity.ok("查詢成功");
    }
    
    @GetMapping("/search")
    public ResponseEntity<?> searchUsers(
            @RequestParam 
            @NotBlank(message = "關鍵字不能為空") 
            String keyword,
            
            @RequestParam 
            @Min(value = 1) 
            @Max(value = 100) 
            Integer page) {
        return ResponseEntity.ok("搜尋成功");
    }
}
```

**重點:** 驗證方法參數時，**必須**在 Controller 類上加 `@Validated` 註解，否則驗證不會生效。

#### 場景 2: 分組驗證
```java
// 定義驗證分組介面
public interface CreateGroup {}
public interface UpdateGroup {}

public class UserDTO {
    
    @NotNull(groups = UpdateGroup.class, message = "更新時 ID 不能為空")
    private Long id;
    
    @NotBlank(groups = {CreateGroup.class, UpdateGroup.class})
    private String username;
    
    @NotBlank(groups = CreateGroup.class, message = "創建時密碼不能為空")
    private String password;
}

// Controller 中使用
@PostMapping
public ResponseEntity<?> createUser(
        @Validated(CreateGroup.class) @RequestBody UserDTO userDTO) {
    // 只驗證屬於 CreateGroup 的規則
    return ResponseEntity.ok("創建成功");
}

@PutMapping("/{id}")
public ResponseEntity<?> updateUser(
        @Validated(UpdateGroup.class) @RequestBody UserDTO userDTO) {
    // 只驗證屬於 UpdateGroup 的規則
    return ResponseEntity.ok("更新成功");
}
```

**重點:** 只有 `@Validated` 支援分組驗證，`@Valid` 不支援。

### 3.4 實際應用建議
```java
@RestController
@RequestMapping("/api/users")
@Validated  // 加上此註解以支援方法參數驗證
public class UserController {
    
    // 使用 @Valid 驗證請求體
    @PostMapping
    public ResponseEntity<?> createUser(@Valid @RequestBody UserDTO userDTO) {
        return ResponseEntity.ok("創建成功");
    }
    
    // 使用 @Validated 驗證請求體 + 分組
    @PutMapping
    public ResponseEntity<?> updateUser(
            @Validated(UpdateGroup.class) @RequestBody UserDTO userDTO) {
        return ResponseEntity.ok("更新成功");
    }
    
    // 驗證 @PathVariable 和 @RequestParam
    @GetMapping("/{id}")
    public ResponseEntity<?> getUser(
            @PathVariable @Min(1) Long id,
            @RequestParam @NotBlank String type) {
        return ResponseEntity.ok("查詢成功");
    }
}
```

**總結建議:**
- 驗證 `@RequestBody` 物件 → 使用 `@Valid`（無分組需求）或 `@Validated`（有分組需求）
- 驗證 `@PathVariable` 或 `@RequestParam` → 必須使用 `@Validated`（類上）+ 驗證註解（參數上）
- 嵌套物件驗證 → 使用 `@Valid`
- 需要分組驗證 → 使用 `@Validated`

## 四、完整實作範例

### 4.1 定義 DTO
```java
import jakarta.validation.constraints.*;
import jakarta.validation.Valid;
import java.time.LocalDate;
import java.util.List;

public class UserDTO {
    
    @NotNull(groups = UpdateGroup.class)
    private Long id;
    
    @NotBlank(message = "使用者名稱不能為空")
    @Size(min = 3, max = 20, message = "使用者名稱長度必須在 3-20 之間")
    private String username;
    
    @NotBlank(message = "密碼不能為空", groups = CreateGroup.class)
    @Size(min = 6, message = "密碼長度至少為 6")
    private String password;
    
    @NotBlank(message = "Email 不能為空")
    @Email(message = "Email 格式不正確")
    private String email;
    
    @NotNull(message = "年齡不能為空")
    @Min(value = 18, message = "年齡必須大於等於 18")
    @Max(value = 120, message = "年齡必須小於等於 120")
    private Integer age;
    
    @Past(message = "生日必須是過去的日期")
    private LocalDate birthDate;
    
    @Pattern(regexp = "^09\\d{8}$", message = "手機號碼格式不正確")
    private String phoneNumber;
    
    @Valid  // 嵌套物件驗證
    private AddressDTO address;
    
    @Valid
    private List<PhoneDTO> phones;
    
    // Getter 和 Setter
}

public class AddressDTO {
    @NotBlank(message = "城市不能為空")
    private String city;
    
    @NotBlank(message = "街道不能為空")
    private String street;
    
    @Pattern(regexp = "^\\d{5}$", message = "郵遞區號必須是 5 位數字")
    private String zipCode;
}
```

### 4.2 定義 Controller
```java
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.*;
import jakarta.validation.Valid;
import jakarta.validation.constraints.*;

@RestController
@RequestMapping("/api/users")
@Validated  // 啟用方法參數驗證
public class UserController {
    
    // 基本的請求體驗證
    @PostMapping
    public ResponseEntity<?> createUser(@Valid @RequestBody UserDTO userDTO) {
        // 驗證失敗會拋出 MethodArgumentNotValidException
        return ResponseEntity.ok("用戶創建成功");
    }
    
    // 分組驗證
    @PostMapping("/v2")
    public ResponseEntity<?> createUserV2(
            @Validated(CreateGroup.class) @RequestBody UserDTO userDTO) {
        return ResponseEntity.ok("用戶創建成功");
    }
    
    @PutMapping("/{id}")
    public ResponseEntity<?> updateUser(
            @PathVariable @Min(1) Long id,
            @Validated(UpdateGroup.class) @RequestBody UserDTO userDTO) {
        return ResponseEntity.ok("用戶更新成功");
    }
    
    // 方法參數驗證
    @GetMapping("/{id}")
    public ResponseEntity<?> getUser(
            @PathVariable 
            @Min(value = 1, message = "ID 必須大於 0") 
            Long id) {
        // 驗證失敗會拋出 ConstraintViolationException
        return ResponseEntity.ok("查詢成功");
    }
    
    @GetMapping("/search")
    public ResponseEntity<?> searchUsers(
            @RequestParam 
            @NotBlank(message = "關鍵字不能為空") 
            String keyword,
            
            @RequestParam(defaultValue = "1")
            @Min(value = 1, message = "頁碼必須大於 0")
            @Max(value = 100, message = "頁碼不能超過 100")
            Integer page,
            
            @RequestParam(defaultValue = "10")
            @Min(value = 1)
            @Max(value = 50)
            Integer size) {
        return ResponseEntity.ok("搜尋成功");
    }
    
    @DeleteMapping("/{id}")
    public ResponseEntity<?> deleteUser(
            @PathVariable @Positive Long id,
            @RequestParam @NotBlank String reason) {
        return ResponseEntity.ok("刪除成功");
    }
}
```

## 五、異常處理

### 5.1 兩種驗證異常

| 異常類型 | 觸發條件 | 驗證目標 |
|---------|---------|---------|
| **MethodArgumentNotValidException** | 使用 `@Valid` 或 `@Validated` 驗證 `@RequestBody` 失敗 | 請求體物件 |
| **ConstraintViolationException** | 驗證方法參數（`@PathVariable`, `@RequestParam`）失敗 | 單個方法參數 |

### 5.2 全局異常處理器
```java
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;
import org.springframework.http.ResponseEntity;
import org.springframework.http.HttpStatus;
import jakarta.validation.ConstraintViolationException;
import java.util.*;

@RestControllerAdvice
public class GlobalExceptionHandler {
    
    /**
     * 處理 @RequestBody 驗證失敗
     * 當使用 @Valid 或 @Validated 驗證請求體物件失敗時觸發
     */
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, Object>> handleMethodArgumentNotValid(
            MethodArgumentNotValidException ex) {
        
        Map<String, String> fieldErrors = new HashMap<>();
        
        // 收集所有欄位錯誤
        ex.getBindingResult().getFieldErrors().forEach(error -> {
            String fieldName = error.getField();
            String errorMessage = error.getDefaultMessage();
            fieldErrors.put(fieldName, errorMessage);
        });
        
        Map<String, Object> response = new HashMap<>();
        response.put("status", "error");
        response.put("message", "請求體驗證失敗");
        response.put("errors", fieldErrors);
        response.put("timestamp", new Date());
        
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(response);
    }
    
    /**
     * 處理 @PathVariable 和 @RequestParam 驗證失敗
     * 當使用 @Validated 驗證方法參數失敗時觸發
     */
    @ExceptionHandler(ConstraintViolationException.class)
    public ResponseEntity<Map<String, Object>> handleConstraintViolation(
            ConstraintViolationException ex) {
        
        Map<String, String> paramErrors = new HashMap<>();
        
        // 收集所有參數錯誤
        ex.getConstraintViolations().forEach(violation -> {
            String paramName = violation.getPropertyPath().toString();
            // 只保留參數名稱部分（移除方法名）
            paramName = paramName.substring(paramName.lastIndexOf('.') + 1);
            
            String errorMessage = violation.getMessage();
            paramErrors.put(paramName, errorMessage);
        });
        
        Map<String, Object> response = new HashMap<>();
        response.put("status", "error");
        response.put("message", "請求參數驗證失敗");
        response.put("errors", paramErrors);
        response.put("timestamp", new Date());
        
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(response);
    }
}
```

### 5.3 異常範例

#### MethodArgumentNotValidException 觸發情況
```java
// Request
POST /api/users
Content-Type: application/json

{
  "username": "",     // 驗證失敗: @NotBlank
  "age": 15          // 驗證失敗: @Min(18)
}

// Response
{
  "status": "error",
  "message": "請求體驗證失敗",
  "errors": {
    "username": "使用者名稱不能為空",
    "age": "年齡必須大於等於 18"
  },
  "timestamp": "2025-11-14T10:30:00.000+00:00"
}
```

#### ConstraintViolationException 觸發情況
```java
// Request
GET /api/users/0?keyword=&page=101

// Response
{
  "status": "error",
  "message": "請求參數驗證失敗",
  "errors": {
    "id": "ID 必須大於 0",
    "keyword": "關鍵字不能為空",
    "page": "頁碼不能超過 100"
  },
  "timestamp": "2025-11-14T10:30:00.000+00:00"
}
```

## 七、常見問題與注意事項

### 7.1 常見錯誤

#### 錯誤 1: 忘記在類上加 @Validated
```java
@RestController
@RequestMapping("/api/users")
// ❌ 缺少 @Validated，方法參數驗證不會生效
public class UserController {
    
    @GetMapping("/{id}")
    public ResponseEntity<?> getUser(@PathVariable @Min(1) Long id) {
        // 驗證不會執行，不會拋出任何異常
        return ResponseEntity.ok("查詢成功");
    }
}

// ✅ 正確寫法
@RestController
@RequestMapping("/api/users")
@Validated  // 必須加這個註解
public class UserController {
    
    @GetMapping("/{id}")
    public ResponseEntity<?> getUser(@PathVariable @Min(1) Long id) {
        // 現在驗證會正常執行
        return ResponseEntity.ok("查詢成功");
    }
}
```

#### 錯誤 2: 嵌套物件忘記加 @Valid
```java
// ❌ 錯誤寫法
public class UserDTO {
    @NotBlank
    private String name;
    
    // 缺少 @Valid，address 內部的驗證不會執行
    private AddressDTO address;
}

// ✅ 正確寫法
public class UserDTO {
    @NotBlank
    private String name;
    
    @Valid  // 必須加 @Valid
    private AddressDTO address;
}
```

#### 錯誤 3: 混淆 @NotNull、@NotEmpty、@NotBlank
```java
public class UserDTO {
    // ❌ String 使用 @NotNull 無法阻止空字串 ""
    @NotNull
    private String username;  // "" 會通過驗證
    
    // ✅ String 應該使用 @NotBlank
    @NotBlank
    private String username;  // null、""、"  " 都不會通過驗證
    
    // ✅ Collection 使用 @NotEmpty
    @NotEmpty
    private List<String> tags;  // null 和空集合都不會通過驗證
}
```

### 7.2 最佳實踐

1. **統一錯誤訊息格式**
   - 使用全局異常處理器統一處理驗證錯誤
   - 提供清晰的錯誤訊息給前端

2. **善用分組驗證**
   - 同一個 DTO 在不同場景可能需要不同驗證規則
   - 避免為不同場景創建重複的 DTO

3. **合理使用自定義驗證**
   - 複雜的業務規則使用自定義驗證註解
   - 提高代碼可讀性和可維護性

4. **注意驗證順序**
   - 先驗證基本格式（@NotBlank、@Size）
   - 再驗證業務規則（自定義驗證）

5. **效能考量**
   - 避免在驗證器中進行資料庫查詢
   - 複雜驗證邏輯應該在 Service 層處理

## 八、快速查找表

### 8.1 選擇驗證註解

| 需求 | 使用註解 |
|-----|---------|
| 驗證請求體物件 | `@Valid` 或 `@Validated` |
| 驗證 URL 路徑參數 | 類上 `@Validated` + 參數上驗證註解 |
| 驗證查詢參數 | 類上 `@Validated` + 參數上驗證註解 |
| 嵌套物件驗證 | 欄位上加 `@Valid` |
| 分組驗證 | `@Validated(Group.class)` |

### 8.2 驗證註解選擇

| 數據類型 | 推薦註解 |
|---------|---------|
| String（不能空白） | `@NotBlank` |
| String（允許空白但不能 null） | `@NotNull` |
| Collection/Map/Array | `@NotEmpty` |
| 數值範圍 | `@Min`, `@Max` |
| 數值正負 | `@Positive`, `@Negative` |
| 字串長度 | `@Size` |
| 字串格式 | `@Pattern` |
| Email | `@Email` |
| 日期過去/未來 | `@Past`, `@Future` |

## 九、總結

### 核心要點

1. **@Valid vs @Validated**
   - 驗證 `@RequestBody` → 兩者都可以
   - 驗證方法參數 → 只能用 `@Validated`（類上）
   - 需要分組驗證 → 只能用 `@Validated`
   - 嵌套物件驗證 → 必須用 `@Valid`

2. **兩種異常**
   - `MethodArgumentNotValidException` → 請求體驗證失敗
   - `ConstraintViolationException` → 方法參數驗證失敗

3. **必須記住**
   - 驗證方法參數時類上必須加 `@Validated`
   - 嵌套物件必須加 `@Valid`
   - String 空值檢查用 `@NotBlank`
   - 必須配置全局異常處理器
   - `PathVariable` 及 `RequestParm` 通常會使用 `@Validated` 進行單一參數的驗證
   - `RequestBody` 在 request 的參數中通常會用 `@Valid` 驗證

### 典型使用模式
```java
@RestController
@RequestMapping("/api/users")
@Validated  // 1. 類上加 @Validated（必須）
public class UserController {
    
    // 2. 請求體用 @Valid
    @PostMapping
    public ResponseEntity<?> createUser(@Valid @RequestBody UserDTO userDTO) {
        return ResponseEntity.ok("創建成功");
    }
    
    // 3. 方法參數直接加驗證註解
    @GetMapping("/{id}")
    public ResponseEntity<?> getUser(@PathVariable @Min(1) Long id) {
        return ResponseEntity.ok("查詢成功");
    }
    
    // 4. 分組驗證用 @Validated
    @PutMapping
    public ResponseEntity<?> updateUser(
            @Validated(UpdateGroup.class) @RequestBody UserDTO userDTO) {
        return ResponseEntity.ok("更新成功");
    }
}
```

## Reference

[Day 16 - Spring Boot 資料驗證 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10275699)

[Jakarta Bean Validation](https://jakarta.ee/specifications/bean-validation/)

[Spring Validation 官方文檔](https://docs.spring.io/spring-framework/reference/core/validation/beanvalidation.html)
