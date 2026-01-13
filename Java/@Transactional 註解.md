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

定義 transaction method 之間如何互相影響

以預設的傳播機制來說，若是當前存在 transaction，則加入該 transaction，否則創建新的 transaction。



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


## Reference

- [Spring Boot 中的 @Transactional：輕鬆駕馭資料庫交易 | Cash Wu Geek](https://blog.cashwu.com/blog/2024/spring-boot-transactional-usage-and-best-practices)