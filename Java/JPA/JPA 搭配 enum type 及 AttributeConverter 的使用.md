---
tags:
  - Java
  - JPA
  - is/evergreen/substantiated
---
## Why we need enum 

>在 JPA 中，enum 類型可以直接映射到數據庫欄位，提供類型安全的數據表示方式。JPA 提供兩種主要的映射策略。

1. `@Enumerated(EnumType.ORDINAL)`
2. `@Enumerated(EnumType.STRING)`

### ORDINAL 

ORDINAL 會將 enum 的狀態以順序儲存到 db 中

```java
public enum Status {
    PENDING,    // 0
    APPROVED,   // 1
    REJECTED    // 2
}

@Entity
public class Order {
    @Enumerated(EnumType.ORDINAL)  // 或省略不寫，默認就是 ORDINAL
    private Status status;
}
```

以上面的程式碼來說，當在程式碼中使用 `Status.PENDING` 存入 db 中時，就會存入`0`

### String

```java
@Entity
public class Order {
    @Enumerated(EnumType.STRING)
    private Status status;
}
```

以上面的程式碼來說，db 儲存的會是 `PENDING`, `APPROVED` 等字串

## AttributeConverter

以上面的狀況來說，雖然通常會透過 `Enumterated.STRING` 將 enum type 儲存到 db 中，但因為 enum 的 naming convention 通常會是全大寫的形式，如果要在業務邏輯中使用的話可能較不適合。

這時候我們可以自訂 enum 對應要儲存到 db 中的值，並搭配 jpa 提供的 `AttributeConverter<X, Y>` 來做處理

我們可以先在 enum 中寫好從自訂值轉為 enum 的 function `fromxxx` 。

```java
@AllArgsConstructor
@Getter
public enum PaymentMethod {
    CREDIT_CARD("CC"),
    DEBIT_CARD("DC"),
    PAYPAL("PP"),
    CASH("CASH");
    
    private final String code;
    
    public static PaymentMethod fromCode(String code) {
        for (PaymentMethod method : values()) {
            if (method.code.equals(code)) {
                return method;
            }
        }
        throw new IllegalArgumentException("Unknown code: " + code);
    }
}
```


接著撰寫 converter 去實作 `AttributeConverter<X, Y>` X 填寫的是 enum，Y 填寫的是要轉換的型態，因為這裡要轉換成 "CC" "DC" 等 code，所以是填寫 `String`  

再來就是實作 `convertToDatabaseColumn` 與 `convertToEntityAttribute` 兩個 function 來讓 JPA 協助進行 enum type 的轉換。j

當實作完成後，在 converter 上面加上 `autoApply = true` 的註解就會讓 JPA 在遇到這個 enum 使用時，自動在自訂值與 enum 之間做轉換。

```java
@Converter(autoApply = true)
public class PaymentMethodConverter 
        implements AttributeConverter<PaymentMethod, String> {
    
    @Override
    public String convertToDatabaseColumn(PaymentMethod method) {
        return method == null ? null : method.getCode();
    }
    
    @Override
    public PaymentMethod convertToEntityAttribute(String code) {
        return code == null ? null : PaymentMethod.fromCode(code);
    }
}

@Entity
public class Payment {
    // 不需要 @Enumerated，因為 Converter 會自動應用
    private PaymentMethod method;
}
```

在 service 層進行撈取的時候就可以直接做使用

```java
@Service
public class PaymentService {
    
    @Autowired
    private PaymentRepository paymentRepository;
    
    /**
     * 根據 ID 查詢單筆付款記錄
     * JPA 會自動使用 PaymentMethodConverter 將資料庫的 'CC', 'DC' 等值
     * 轉換回 PaymentMethod enum
     */
    public Payment getPaymentById(Long id) {
        Payment payment = paymentRepository.findById(id)
            .orElseThrow(() -> new EntityNotFoundException("Payment not found"));
        
        // 這裡的 payment.getMethod() 已經是 PaymentMethod enum 類型了
        // Converter 已經自動完成轉換：'CC' -> PaymentMethod.CREDIT_CARD
        PaymentMethod method = payment.getMethod();
        
        System.out.println("付款方式: " + method);  // 輸出：CREDIT_CARD
        System.out.println("付款代碼: " + method.getCode());  // 輸出：CC
        System.out.println("顯示名稱: " + method.getDisplayName());  // 輸出：信用卡
        
        return payment;
    }
    
    /**
     * 查詢所有付款記錄
     * 批量查詢時，每筆記錄的 enum 都會自動轉換
     */
    public List<Payment> getAllPayments() {
        List<Payment> payments = paymentRepository.findAll();
        
        // 遍歷時可以直接使用 enum
        for (Payment payment : payments) {
            PaymentMethod method = payment.getMethod();
            
            // 可以直接用 enum 做判斷
            if (method == PaymentMethod.CREDIT_CARD) {
                System.out.println("這是信用卡付款");
            }
        }
        
        return payments;
    }
    
    /**
     * 根據付款方式查詢
     * 注意：Repository 方法的參數直接使用 enum 類型
     * JPA 會用 Converter 將 enum 轉換成資料庫值去查詢
     */
    public List<Payment> getPaymentsByMethod(PaymentMethod method) {
        // 傳入 PaymentMethod.CREDIT_CARD 時
        // Converter 會將它轉換成 'CC' 去資料庫查詢
        return paymentRepository.findByMethod(method);
    }
    
    /**
     * 統計各種付款方式的使用次數
     */
    public Map<PaymentMethod, Long> getPaymentMethodStatistics() {
        List<Payment> allPayments = paymentRepository.findAll();
        
        // 可以直接對 enum 做分組統計
        return allPayments.stream()
            .collect(Collectors.groupingBy(
                Payment::getMethod,  // 直接使用 enum
                Collectors.counting()
            ));
    }
    
    /**
     * 示範：處理可能為 null 的情況
     */
    public void processPayment(Long paymentId) {
        Payment payment = paymentRepository.findById(paymentId)
            .orElseThrow(() -> new EntityNotFoundException("Payment not found"));
        
        PaymentMethod method = payment.getMethod();
        
        // Converter 的 convertToEntityAttribute 處理了 null 的情況
        // 所以這裡要防範 null
        if (method == null) {
            throw new IllegalStateException("Payment method cannot be null");
        }
        
        // 根據不同的付款方式執行不同的邏輯
        switch (method) {
            case CREDIT_CARD:
            case DEBIT_CARD:
                System.out.println("處理卡片付款");
                break;
            case PAYPAL:
                System.out.println("處理 PayPal 付款");
                break;
            case CASH:
                System.out.println("處理現金付款");
                break;
        }
    }
}
```

## Reference

- [JPA Specification - EnumType](https://jakarta.ee/specifications/persistence/3.1/jakarta-persistence-spec-3.1.html) 
- [Hibernate ORM User Guide - Basic Types](https://docs.jboss.org/hibernate/orm/6.0/userguide/html_single/Hibernate_User_Guide.html#basic) 
- [Spring Data JPA Documentation](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
- [JPA AttributeConverter：了解怎麼讓 Enum 與資料庫欄位的轉換全自動化 | by MwA | Medium](https://medium.com/@ymwkac/jpa-attributeconverter-%E4%BA%86%E8%A7%A3%E6%80%8E%E9%BA%BC%E8%AE%93-enum-%E8%88%87%E8%B3%87%E6%96%99%E5%BA%AB%E6%AC%84%E4%BD%8D%E7%9A%84%E8%BD%89%E6%8F%9B%E5%85%A8%E8%87%AA%E5%8B%95%E5%8C%96-1d8f6db1a651)