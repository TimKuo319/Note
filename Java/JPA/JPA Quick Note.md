
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