---
tags:
  - Java
  - ORM
---
## Startup

透過 JPA，我們可以透過一些常見的 annotation 去對 entity 做關聯，以下是基本的使用方式。

## Usage

### OneToOne

下面是 OneToOne 的程式碼，從兩個 entity 中可以看到，上面個別加上了 `@OneToOne` 的 annotation，代表的是兩者是一對一的關聯關係。`@OneToOne` 的 `mappedBy` 屬性是指 *受到哪一個 entity 管理* ，被管理的 entity 稱為 *被控端* ，而管理的則稱為 *主控端* ，以下面的例子來說，`User` 就是主控端，`Profile` 就是被控端。

`@JoinColumn` 的話則是會在 User entity 中加上名為 `profile_id` 的欄位，參考的是 `Profile` table 中的 `id` 欄位。

>[!info] 
>1. mappedBy 要填寫的是在 entity 中的屬性名稱
>2. @JoinColumn 會使得 entity 在資料庫中加上該外鍵欄位，通常是加在被控端上管理


```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "profile_id", referencedColumnName = "id")
    private Profile profile;

    // Getters and Setters
}
```

```java
@Entity
public class Profile {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String address;

    @OneToOne(mappedBy = "profile")
    private User user;

    // Getters and Setters
}

```

>[!info] 
>mappedby 填入的對象是在 owner 的屬性名稱


## Owing Side(控制端) vs Inverse Side(被控端)

在 JPA 的關聯關係中，存在著 Owing Side 與 Inverse Side。

owing Side 是持有外鍵屬性，負責更新 join table、 join column 的一端。而 inverse Side 則是作為關聯查詢用途，並不會參與 join table 的更動，通常會以 `@mappedBy` 來指向 owing side。(上面程式碼中的 Profile 就是 inverse side)

而為何需要理解 owing side 與 inverse side 呢？

就如同前面所提到的，只有 owing side 本身會參與 join table 或 join column 的一端，所以在程式語言上如果在更新資料時是更新 inverse side 的話，實際改變的只是記憶體中的物件狀態，並不會去影響到 DB 中彼此 join column 的欄位，進而導致資料出錯。是使用上需要注意的部分。

## Reference

[Demystifying JPA Many-to-Many Relationships: Focus on Owning and Inverse Sides | by Rashid Mammadli | Medium](https://medium.com/@devrashid/demystifying-jpa-many-to-many-relationships-focus-on-owning-and-inverse-sides-54ba0a21af0b)

