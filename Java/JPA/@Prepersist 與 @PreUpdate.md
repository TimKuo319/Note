
## What is @Prepersist and @PreUpdate, why we need it

Prepersist 與 PreUpdate 是在資料被塞入 DB 前進行 entity 的初始化，常見會用於產生像是 timestamp 等時間戳記。

以記錄創建來說，通常會需要紀錄創建與修改的時間，而這個時候就可以在程式中透過，`@PrePersist` 及 `@PreUpdate`  等 annotation 來產生資料後存入 db 中。

```java 
import jakarta.persistence.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "products")
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @Column(name = "created_at", nullable = false, updatable = false)
    private LocalDateTime createdAt;

    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    // --- Lifecycle Methods ---

    @PrePersist
    protected void onCreate() {
        // 當資料準備存入資料庫前，設定目前的系統時間
        this.createdAt = LocalDateTime.now();
        this.updatedAt = LocalDateTime.now();
    }

    @PreUpdate
    protected void onUpdate() {
        // 當資料被更新前，刷新更新時間
        this.updatedAt = LocalDateTime.now();
    }

    // Getters and Setters...
}
```

## 選型考量

但對於像是 timestamp 等方法在 db 中同樣也有提供使用的方式，那兩者的差異為何？

###  DB Side

#### 優點

1. **絕對一致性**：無論是透過 App、DBeaver 還是手動下 SQL 指令改資料，時間戳都會被更新。
    
2. **效能極致**：資料庫原生支援，減少了 App 的運算。
    
#### 缺點

1. **Entity 狀態不同步**：當你執行 `repo.save(entity)` 後，Entity 物件裡的 `createdAt` 會是 `null`，除非你手動執行 `em.refresh(entity)`。這在需要「存完馬上回傳資料」的 API 情境下很麻煩。
    
2. **資料庫依賴**：不同 DB 的語法不同（例如 MySQL 和 Oracle 寫法有差），導致遷移資料庫時需要改 Schema。
    
3. **時區問題**：如果 DB 伺服器跟 App 伺服器時區設定不一致，會導致時間混亂。


### 程式面

#### 優點

1. **物件即時可用**：存檔後，Entity 物件內的屬性已經有值，不需重新查詢資料庫。
    
2. **跨資料庫一致**：無論底層換成什麼資料庫，行為都一致。
    
3. **邏輯彈性**：你可以根據 App 的業務邏輯（例如：特定使用者操作才更新時間）進行判斷。
#### 缺點

1. **後門風險**：如果有人直接用資料庫工具（如：Navicat）修改資料，`updated_at` 就不會更新。
    
2. **時間漂移**：在多台伺服器負載平衡的情境下，如果各伺服器時間沒有同步（NTP），時間戳可能會有幾秒鐘的誤差。


## Summary

| **特性**   | **DB 自動生成**          | **JPA / App 生成 (推薦)** |
| -------- | -------------------- | --------------------- |
| **可維護性** | 邏輯隱藏在 Schema 裡，難追蹤   | 邏輯在代碼中，一目了然           |
| **即時性**  | 存入後須 `refresh` 才拿得到值 | 存入當下物件即有值             |
| **安全性**  | 即使手動改 DB 也會更新        | 手動改 DB 則無反應           |
| **擴充性**  | 難以記錄「誰修改的」           | 容易整合 Security 記錄使用者   |

## Reference

- [【原创】JPA中@PrePersist和@PreUpdate的用法-CSDN博客](https://blog.csdn.net/DCTANT/article/details/115583053)

