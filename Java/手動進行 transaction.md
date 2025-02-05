---
tags:
  - database
---
## Why? 

雖然在 spring boot 中可以透過 `@Transactional` 來自動幫我們進行 transaction 的 commit 以及 rollback，但若是需要比較精細的 transaction 或是針對特定的異常做額外處理時，可能就會需要手動進行 transaction 的處理。

## Usage

在 spring 框架中，主要透過 `PlatformTransactionManager` 和 `TransactionDefinition` 這兩個主要的 interface 來協助進行手動 transaction 處理。

### PlatformTransactionManager

`PlatformTransactionManager` 定義了 transaction 的基本方法
>	- `TransactionStatus getTransaction(@Nullable TransactionDefinition definition)`
	- `void commit(TransactionStatus status)`
	- `void rollback(TransactionStatus status)`

並讓這些方法的實際使用依照不同場境有不同的實作。讓開發者可以依照自身使用場景選擇對應的 manager。

>- **DataSourceTransactionManager**：用於管理基於 JDBC 的事務。
>- **JpaTransactionManager**：用於管理基於 JPA 的事務。
>- **HibernateTransactionManager**：用於管理基於Hibernate 的事務。
### TransactionDefinition

`TransactionDefinition` 用來定義 transaction 的屬性。

- Propagation Behavior - 定義 transaction 的傳播方式
- Isolation level - 指定 transaction 的隔離層級
- Timeout 
- Read-Only - 指定 transaction 使否唯讀

### TransactionStatus

TransactionStatus 表示 transaction 的執行狀態，透過 TransactionStatus 可以讓開發者查詢事務執行的狀態。

透過以上三者的配合 `PlatformTransactionManager` 負責處理事務的操作，`TransactionDefinition` 負責定義事務的屬性，而 `TransactionStatus` 則是表示 transaction 的執行狀態，來完成手動執行 transaction 的任務。

## Usage

```java
package com.example.demo.service;  
  
import com.example.demo.dto.CreateUserDto;  
import com.example.demo.entity.User;  
import com.example.demo.repository.UserRepository;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.boot.autoconfigure.AutoConfiguration;  
import org.springframework.stereotype.Service;  
import org.springframework.transaction.PlatformTransactionManager;  
import org.springframework.transaction.TransactionDefinition;  
import org.springframework.transaction.TransactionStatus;  
import org.springframework.transaction.support.DefaultTransactionDefinition;  
  
@Service  
public class UserService {  
  
    @Autowired  
    private UserRepository userRepository;  
  
    @Autowired  
	// 注入 manager
    private PlatformTransactionManager transactionManager;  
  
    public void createUser(CreateUserDto user) {  
		// 透過 definition 定義屬性
        DefaultTransactionDefinition def = new DefaultTransactionDefinition();  
        def.setName("creteUserTransaction");  
        def.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRED);  
  
        TransactionStatus status = transactionManager.getTransaction(def);  
  
        User newUser = new User();  
        newUser.setUsername("guo");  
        newUser.setEmail("test@gmail.com");  
  
        try{  
            userRepository.save(newUser);  
            int sum = 1 / 0;  
            transactionManager.commit(status);  
        } catch (Exception e) {  
            transactionManager.rollback(status);  
            throw e;  
        }  
  
    }  
}
```

## Reference



