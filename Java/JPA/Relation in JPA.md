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
>@JoinColumn 會使得 entity 在資料庫中加上該外鍵欄位，通常是加在被控端上管理


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



