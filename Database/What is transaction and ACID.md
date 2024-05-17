---
alias:
  - ACID principle
---

---

## Transaction

>Transaction，中文翻作交易或事務，是資料庫執行過程中的一個「邏輯單位」，一個 transaction 中包含多個對資料庫操作的行為，每個 transaction 有兩種可能的結局：全部執行成功 or 全部不執行（只要其中一個行為失敗就全部回滾

transaction的存在主要是為了確保兩件事情
+ 操作的嚴謹性 -> 失敗時即==rollback==
+ 作業執行間的獨立性

### 常見指令

+ START TRANSACTION
	+ 建立一個transaction
+ COMMIT
	+ 在未COMMIT前，transaction的變更操作(INSERT、UPDATE、DELETE)等，都`僅存在資料庫的buffer中`，只有commit被執行後，所做的變更才會真正的儲存到資料庫中，讓其他用戶或連線看見變更。
	+ transaction操作成功結束時進行commit
+ ROLLBACK
	+ transaction操作失敗時進行rollback

>[!info]
>現今我們對資料庫進行修改操作時，可能會發現即使不加上`COMMIT`，也能夠讓其他用戶或連線看見資料變更，原因是因為有些資料庫會預設`AUTO_COMMMIT`，也就是在執行這些變更操作時自動執行COMMIT。
>
>以mysql來說，可以透過以下方式查看是否有`AUTO_COMMIT`。
>``` sql
>select @@autocommit;

## ACID

ACID是RDBMS中確保資料一致性和可靠性的重要原則。`但不僅RDBMS有這個特性`，有些NoSQL也有支援這個規則。ex: `MongoDB`

1. 原子性 (Atomicity) : 確保一個`Transaction`被看作不可分割的操作，也就是這個Transaction中的所有操作要不是全部成功，就是全部失敗。且在失敗時必須回復到Transaction前的狀態。以確保data的consistency。

2. 一致性 (Consistency) : 確保Transaction在開始和完成的時候，將數據從一個consistency的狀態轉移到另一個consistency的狀態。也就代表Transaction的執行並不會違反整個database的約束。

3. 隔離性 (Isolation) : 確保多個Transaction同時執行時，每個Transaction看起來好像都在單獨執行，而不會互相影響。這可以防止`Race Condition`、`non-repeatable reads`等等問題。

4. 持久性 (Durability) : 確保一旦Transaction成功執行，其所作的變更在系統故障或crash後仍然會存在。通常涉及將Data寫入Stroage，像是Disk。


*Transaction: 指的是一連串相關的操作或task*

## Reference
[Database Transaction & ACID - 莫力全 Kyle Mo - Medium](https://oldmo860617.medium.com/database-transaction-acid-156a3b75845e)
