

## JPA Auditing

JPA 提供了審計功能，可以自動追蹤實體的創建時間、修改時間、創建者和修改者等資訊。主要有兩套註解：

1. **Spring Data JPA 註解**：`@CreatedDate`、`@LastModifiedDate`、`@CreatedBy`、`@LastModifiedBy`
2. **Hibernate 註解**：`@CreationTimestamp`、`@UpdateTimestamp`

## Spring Data JPA 審計註解

1. 啟用審計功能
在主類或配置類上加上 `@EnableJpaAuditing`：

```java
@SpringBootApplication
@EnableJpaAuditing
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

2. 在 Entity 上使用審計註解
```java
@Entity
@EntityListeners(AuditingEntityListener.class)  // 必須加這個監聽器
public class Article {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String title;
    
    @Column(nullable = false)
    private String content;
    
    // 創建時間 - 只在新增時設定一次
    @CreatedDate
    @Column(nullable = false, updatable = false)
    private LocalDateTime createdDate;
    
    // 最後修改時間 - 每次更新時都會改變
    @LastModifiedDate
    @Column(nullable = false)
    private LocalDateTime lastModifiedDate;
    
    // Getters and Setters
}
```



- [ ] @CreateDate 
- [ ] @LastModifiedDate 
- [ ] @CreateTimeStamp
- [ ] @LastModifiedTimeStamp


- [ ] jpa auditing annotation

- 延伸
- [ ] postgresql 欄位有類似 CreateDate 或是 LastModifiedDate 的欄位設定嗎