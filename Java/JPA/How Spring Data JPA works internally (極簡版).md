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

2. 如果 repository 是透過 interface 定義的，會採用 `JDK proxy` 來建立實際的 proxy class，如果要代理的目標是 class，則會採用 `CGLIB` 的方式，proxy class 的生成主要是透過 

3. 

## Reference
