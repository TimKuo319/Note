---
tags:
  - Java
  - ORM
---
## What is `JPA`

`JPA` - Java Persistence API，是一種規範，並不包含實作。`Hibernate` 就是 JPA 的 Provider 之一。只要使用的是遵守 JPA 的 ORM Framework，未來就可以自由的替換 Provider 。

在撰寫 `Spring Boot` 的時候應該會常見看到像是 `Spring Data JPA` 這樣的名詞，
而`Spring Data JPA` 是在 JPA 上提供的抽象層，讓開發者可以更方便的使用 JPA 及更輕鬆的開發 JPA Provider。


## JPA vs Spring Data JPA

雖然前面有提到 `Spring Data JPA` 是做更高層次的抽象，那究竟差異在哪裡呢？以純 JPA 來說，如果要使用的話會需要自己撰寫 `entityManager` 來處理

```java
@PersistenceContext
private EntityManager entityManager;

public List<User> findAllUsers() {
    return entityManager.createQuery("SELECT u FROM User u", User.class).getResultList();
}

```

但如果透過 `Spring Data JPA` 的話，只需要定義好介面，再依照官方文件給的方法去進行 method 命名，就能產生出對應的 SQL query 進行資料的查詢。大幅簡化了開發者在使用上的複雜度。

```java
public interface UserRepository extends JpaRepository<User, Long> { }
```


## Terminology

- `Entity` 
	-  透過程式語言來建立 entity，相當於對應到資料庫之中的 table

- `Repository` 

	- 是用來對資料進行查詢的介面。JPA 提供 `JpaRepository`，讓我們可以自動生成 CRUD 操作。
	
	- 只需要繼承 `JpaRepository` 就能不用自己撰寫 CRUD 操作。

- `EntityManager` 
	- 負責所有資料庫之間的互動。
	- 通常不用手動操作

- `Transaction`

	- 確保資料一致性

-  `JPQL (Java Persistence Query Language)`

	- **定義**：JPQL 是 JPA 的查詢語言，類似於 SQL，但它是針對 Java 物件進行操作，而不是資料庫表格。
	- **關鍵點**：你可以使用 JPQL 在 `Entity` 上進行複雜的查詢，這比直接使用 SQL 更有彈性。

- `Cascade 和 Fetch Type`

- `Relationships`

- `Primary Key Generation Strategies`

## Reference

- [JPA Query Methods :: Spring Data JPA](https://docs.spring.io/spring-data/jpa/reference/jpa/query-methods.html#jpa.query-methods.query-creation)

- [(13) 甚麼是 JDBC、ORM、 JPA、ORM框架、Hibernate | by Albert Hg | learning-from-jhipster | Medium](https://medium.com/learning-from-jhipster/13-%E7%94%9A%E9%BA%BC%E6%98%AF-jdbc-orm-jpa-orm%E6%A1%86%E6%9E%B6-hibernate-c762a8c5e112)