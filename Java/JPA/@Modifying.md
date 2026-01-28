

- 用於搭配 `@Query` 進行使用。當 `@Query` 的內容是如 `insert`、`update`、`delete` 等修改語句時，會需要加上 `@Modify` 直接到 db 進行更改資料，如果沒有使用 `@Modifying` spring boot 也會報錯

- 使用時需注意 `persistence context` 的實體同步問題，因為透過 `@Modifying` 會直接到 DB 進行資料更新，會造成 spring 在記憶體中儲存的資料與 DB 不同步的問題，可以參考 `clearAutomatically` 與 `flushAutomatically` 解決




## Reference

[JPA Query Methods :: Spring Data JPA](https://docs.spring.io/spring-data/jpa/reference/jpa/query-methods.html#jpa.modifying-queries)

[Spring Data JPA @Modifying Annotation | Baeldung](https://www.baeldung.com/spring-data-jpa-modifying-annotation)