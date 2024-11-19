---
tags:
  - Java
  - SpringBoot
---

從 `Spring Data JPA` 基本的使用過程中我們可以知道，對於開發者來說，只要定義一個 interface，並且繼承 `JpaRepository` 這個 interface，我們就能夠有基本的 query method 可以使用。

但這個過程是怎麼做到的呢？

## How Spring Data JPA works internally 

就像 [[JPA]] 中提到的，`Spring Data JPA` 是更高層次的封裝。但其實本質上他還是透過純 JPA 去進行操作，才可以達到與資料庫互動的效果，`Spring Data JPA` 其實是默默替我們建立了 proxy class，才讓我們有那些 method 可以呼叫。以下是具體的步驟

1. 在 spring boot 啟動的時候，會先去掃描有 `Repository` 或 `EnableJpaRepository` annotation 標記的 package，這些目標會被視為創建 repository 的目標

2. 如果 repository 是透過 interface 定義的，會採用 `JDK proxy` 來建立實際的 proxy class，如果要代理的目標是 class，則會採用 `CGLIB` 的方式，proxy class 的生成主要是透過 `Spring Data JPA` 中 [RepositoryFactory]([spring-data-commons/src/main/java/org/springframework/data/repository/core/support/RepositoryFactorySupport.java at main · spring-projects/spring-data-commons](https://github.com/spring-projects/spring-data-commons/blob/main/src/main/java/org/springframework/data/repository/core/support/RepositoryFactorySupport.java))  的 `getRepository(Class<T>, RepositoryFragments)` 透過 `ProxyFactory` 去生成，不過目前看 source code 的能力還不夠，之後再慢慢補強。

3. `Spring Data JPA` 透過在初始化的時候去建立 [JpaRepositoryFacotry](https://github.com/spring-projects/spring-data-jpa/blob/main/spring-data-jpa/src/main/java/org/springframework/data/jpa/repository/support/JpaRepositoryFactory.java#L296) ，他會去生成 `SimpleRepository` 作為前面 proxy class 的代理。

4. 透過上面作法生成的 `SimpleJpaRepository` ，也就是 repository 的具體實作類別，當有請求在呼叫我們定義的 interface 中的 method 時，就會透過 proxy class 攔截對 repository interface 的呼叫，透過去判斷 method 決定是否要直接使用 `SimpleRepository` 的預設方法，或是透過 `QueryLookupStrategy` 進行方法的解析，生成對應的 SQL query 後再交由代理的 `SimpleRepository` 中的 `EntityManager` 去執行語法

## Summary

1. 啟動時，掃描並註冊 Repository 介面：`UserRepository extends JpaRepository<User, Long>`

2. 使用 `JpaRepositoryFactory`：
   生成 `SimpleJpaRepository` 作為具體執行目標。
   
3. 使用 `ProxyFactory`：
   生成代理類，將 `SimpleJpaRepository` 注入其中。
   
4. 當請求呼叫 `UserRepository` 時：
   - `Proxy Class` 攔截方法調用。
   - 如果是預定義方法（如 save）：委派給 `SimpleJpaRepository`。
   - 如果是自定義方法（如 findByName）：解析方法名稱生成查詢，然後執行。


## Feedback

從參考的文章、JPA 的 source code 以及和 GPT 的對話，有大致上了解到 JPA 是如何透過定義介面讓我們能夠更方便的使用 hibernate，也是第一次比較認真的去看 source code，雖然還有滿多語法還不太懂，且對於怎麼找檔案之間的關聯還沒有一個頭緒，但至少應該算是個還不錯的開始！ 

從實際看 source code 感覺也看到滿多不同的用法的，從 Factory、Strategy 等結尾可以猜測到大概都是遵循著這些 Design Pattern 去實作的，也是自己要再加強的部分！

## Reference

- [spring data jpa（概述、快速入门、内部原理剖析、查询使用方式）一、概述 1.1   Spring Data - 掘金](https://juejin.cn/post/6999782550910533645) 
- [spring-projects/spring-data-jpa: Simplifies the development of creating a JPA-based data access layer.](https://github.com/spring-projects/spring-data-jpa)