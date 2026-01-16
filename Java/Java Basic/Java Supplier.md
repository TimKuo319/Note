
## What is Supplier

>`Supplier<T>` 是 Java 8 引入的函數式接口（Functional Interface），定義在 `java.util.function` 包中。它代表一個**不接受任何參數，但會返回結果的函數**。

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```

## Usage

```java
// 1. Lambda 表達式寫法
Supplier<String> stringSupplier = () -> "Hello World";
String result = stringSupplier.get(); // "Hello World"

// 2. 方法引用寫法
Supplier<LocalDateTime> timeSupplier = LocalDateTime::now;
LocalDateTime now = timeSupplier.get();

// 3. 傳統匿名類寫法（較少用）
Supplier<Integer> numberSupplier = new Supplier<Integer>() {
    @Override
    public Integer get() {
        return 42;
    }
};
```


## 核心特性

- **延遲執行（Lazy Evaluation）**：只有在調用 `get()` 時才執行邏輯
- **無參數輸入**：不需要任何輸入參數
- **有返回值**：必須返回一個結果

### 延遲建立 instance

```java
@Service
public class UserService {
    private static final Logger log = LoggerFactory.getLogger(UserService.class);
    
    public void processUser(User user) {
        // ❌ 不好的做法：無論是否需要記錄，都會執行字串拼接
        log.debug("Processing user: " + user.getDetailedInfo());
        
        // ✅ 好的做法：只有在 DEBUG 級別啟用時才執行
        log.debug("Processing user: {}", () -> user.getDetailedInfo());
    }
}
```
### Log 優化

透過 supplier 延遲某些 log 啟用

```java
@Service
public class UserService {
    private static final Logger log = LoggerFactory.getLogger(UserService.class);
    
    public void processUser(User user) {
        // ❌ 不好的做法：無論是否需要記錄，都會執行字串拼接
        log.debug("Processing user: " + user.getDetailedInfo());
        
        // ✅ 好的做法：只有在 DEBUG 級別啟用時才執行
        log.debug("Processing user: {}", () -> user.getDetailedInfo());
    }
}
```

## Summary

### 優點

1. **延遲執行**：只在需要時才計算結果
2. **減少資源浪費**：避免不必要的對象創建或計算
3. **提升性能**：在分支邏輯中特別有效
4. **代碼簡潔**：配合 Lambda 表達式更易讀

### 缺點

1. **線程安全**：Supplier 本身不保證線程安全
2. **副作用**：如果 Supplier 中有副作用（如修改共享狀態），需要謹慎使用
3. **調試困難**：Lambda 表達式的堆棧追蹤可能不如普通方法清晰


實務上最常見的方是還是在進行延遲載入等，避免不必要的資源消耗