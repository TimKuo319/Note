---
tags:
  - SpringBoot
---

## What is `@Transactional`, why we need it

>`@Transactional` 是 Jakarta EE（前身是 Java EE）規範中定義的標準化事務管理註解。它告訴容器「這個方法需要在事務環境中執行」。

透過 `@Transactional`，能夠在 function 遇到錯誤時自動將其中的操作回滾來避免資料錯誤。

## Usage

### 基本使用

```java
@Service
public class UserService {
    
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private AccountRepository accountRepository;
    
    // 基本用法：方法級別
    @Transactional
    public void createUser(User user) {
        userRepository.save(user);
        // 如果這裡拋出異常，上面的 save 會回滾
        accountRepository.save(new Account(user.getId()));
    }
}

// 類級別：所有 public 方法都會套用
@Service
@Transactional
public class OrderService {
    
    public void createOrder(Order order) {
        // 自動開啟事務
    }
    
    // 可以覆蓋類級別設定
    @Transactional(readOnly = true)
    public Order findOrder(Long id) {
        // 唯讀事務
    }
}
```

### 重要屬性

#### 1. propagation

>交易傳播行為定義了當一個**已經有交易的方法**，去呼叫**另一個也有交易的方法**時，交易應該如何運作。

- `REQUIRED`
	-  **場景**：`Service A` 的 `methodA()` 呼叫 `Service B` 的 `methodB()`，兩者都有 `@Transactional(propagation = Propagation.REQUIRED)`。
	- **結果**：`methodB()` 會加入 `methodA()` 建立的交易中。它們是**生命共同體**，只要任何一方失敗，整個交易（包含 A 和 B 的操作）都會一起回滾 (Rollback)。
- `REQUIRES_NEW`
	- **場景**：`Service A` 的 `methodA()` 呼叫 `Service B` 的 `methodB()`，`methodB()` 設定了 `@Transactional(propagation = Propagation.REQUIRES_NEW)`。
	- **結果**：當執行到 `methodB()` 時，`methodA()` 的交易會被暫停。Spring 會為 `methodB()` 開啟一個全新的交易。
	    - 如果 `methodB()` 成功，它的交易會獨立提交。
	    - 如果 `methodB()` 失敗，只有 `methodB()` 的交易會回滾 (Rollback)。`methodA()` 的交易不受影響（除非 `methodA()` 沒有處理 `methodB()` 拋出的例外，導致自己也失敗）。
	    - `methodB()` 結束後，`methodA()` 被掛起的交易會恢復執行。
- `REQUIRES_NESTED`
	- 作為巢狀的的 transaction，當自身拋出 exception 時會進行 rollback，但不會影響到外部的 transaction。

```java
@Service
public class PaymentService {
    
    @Autowired
    private LogService logService;
    
    // REQUIRED（預設）：如果當前存在事務，加入該事務；否則創建新事務
    @Transactional(propagation = Propagation.REQUIRED)
    public void processPayment(Payment payment) {
        // 執行支付邏輯
        paymentRepository.save(payment);
        
        // logService 的方法會加入這個事務
        logService.log("Payment processed");
    }
    
    // REQUIRES_NEW：總是創建新事務，暫停當前事務
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void recordAudit(String action) {
        // 即使外層事務回滾，這裡的記錄仍會提交
        auditRepository.save(new Audit(action));
    }
    
    // NESTED：如果當前存在事務，則在嵌套事務內執行
    @Transactional(propagation = Propagation.NESTED)
    public void updateInventory(Long productId, int quantity) {
        // 可以獨立回滾，不影響外層事務
        inventoryRepository.updateStock(productId, quantity);
    }
}
```

### 隔離層級

```java
@Service
public class ReportService {
    
    // READ_COMMITTED：防止髒讀
    @Transactional(isolation = Isolation.READ_COMMITTED)
    public BigDecimal calculateRevenue() {
        // 只能讀取已提交的數據
        return orderRepository.sumTotalAmount();
    }
    
    // REPEATABLE_READ：防止髒讀和不可重複讀
    @Transactional(isolation = Isolation.REPEATABLE_READ)
    public void generateReport() {
        // 同一事務內多次讀取相同數據，結果一致
        List<Order> orders1 = orderRepository.findAll();
        // ... 其他操作
        List<Order> orders2 = orderRepository.findAll();
        // orders1 和 orders2 的內容相同
    }
    
    // SERIALIZABLE：最高隔離級別，防止幻讀
    @Transactional(isolation = Isolation.SERIALIZABLE)
    public void criticalOperation() {
        // 效能最差，但數據一致性最強
    }
}
```


## 注意事項

### 1. 自調用 (Self-invocation) 陷阱

Spring 的事務是透過 **AOP Proxy** 實現的。如果你在同一個類別中，方法 A 調用方法 B，B 的 `@Transactional` 會**直接失效**。

```java
@Service
public class MyService {
    public void methodA() {
        methodB(); // 這裡調用不會觸發事務，因為沒經過代理對象
    }

    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void methodB() { ... }
}
```

**解法：** 將 `methodB` 移到另一個 Service，或透過 `ApplicationContext` 取得代理對象再調用。

### 2. Checked Exception 不會回滾

預設情況下，Spring 只會針對 `RuntimeException` 和 `Error` 進行回滾。如果你拋出的是 `Exception` (Checked Exception)，你需要透過 `rollback` 顯式指定針對特定 exception 進行 rollback：


```java
@Transactional(rollbackFor = Exception.class)
```

## 進階議題

- [ ] 多個 microservice 之間的一致性

## Reference

- [Spring Boot 中的 @Transactional：輕鬆駕馭資料庫交易 | Cash Wu Geek](https://blog.cashwu.com/blog/2024/spring-boot-transactional-usage-and-best-practices)
- [iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/m/articles/10378934)