---
tags:
  - Java
  - ORM
---

- `GeneratedValue` - primary key 的生成策略
	- `IDENTITY` - 相當於 auto increment 
	- `SEQUENCE`
	- `TABLE`

- `JoinColumn`  -  會在他修飾的 entity 中取一個屬性作為現在 entity 的 column

以下面的程式碼來說，`@JoinColumn` 修飾的是 user entity，在沒有指定要以 user 中的哪個屬性作為 column 的狀況下，預設會以目標的 primary key 作為 column，name 代表的則是為這個 column 取名。以下面的例子來說就是 `user_id`

```java
package com.example.demo.model;  
  
  
import jakarta.persistence.*;  
import lombok.Data;  
  
import java.util.ArrayList;  
import java.util.List;  
  
@Entity  
@Data  
@Table(name = "orders")  
public class Order {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long id;  
  
    @ManyToOne  
    @JoinColumn(name = "user_id")  
    private User user;  
  
    @ManyToMany  
    @JoinTable(  
        name = "order_product",  
        joinColumns = @JoinColumn(name = "order_id"),  
        inverseJoinColumns = @JoinColumn(name = "product_id")  
    )  
    private List<Product> products = new ArrayList<>();  
  
}
```
 
- `JoinTable` - 用於多對多關係用來建立中間表

語法架構

```java
@ManyToMany
@JoinTable(
	name = "中間表名稱"
	joinColumns = @JoinColumn(name = "當前實體外鍵名"),
	inverseJoinColumns = @JoinColumn(name = "關聯實體外鍵名“)
)
private List<關聯實體> 屬性名稱;
```

```java
@ManyToMany  
@JoinTable(  
    name = "order_product",  
    joinColumns = @JoinColumn(name = "order_id"),  
    inverseJoinColumns = @JoinColumn(name = "product_id")  
)  
private List<Product> products = new ArrayList<>();
```




- [ ] one to one 
- [ ] many to many
- [ ] many to one
- [ ] @CreateDate 
- [ ] @LastModifiedDate 
- [ ] @CreateTimeStamp
- [ ] @LastModifiedTimeStamp

