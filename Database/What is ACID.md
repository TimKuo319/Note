---
alias:
  - ACID principle
---

---

ACID是RDBMS中確保資料一致性和可靠性的重要原則。`但不僅RDBMS有這個特性`，有些NoSQL也有支援這個規則。ex: `MongoDB`

1. 原子性 (Atomicity) : 確保一個`Transaction`被看作不可分割的操作，也就是這個Transaction中的所有操作要不是全部成功，就是全部失敗。且在失敗時必須回復到Transaction前的狀態。以確保data的consistency。

2. 一致性 (Consistency) : 確保Transaction在開始和完成的時候，將數據從一個consistency的狀態轉移到另一個consistency的狀態。也就代表Transaction的執行並不會違反整個database的約束。

3. 隔離性 (Isolation) : 確保多個Transaction同時執行時，每個Transaction看起來好像都在單獨執行，而不會互相影響。這可以防止`Race Condition`、`non-repeatable reads`等等問題。

4. 持久性 (Durability) : 確保一旦Transaction成功執行，其所作的變更在系統故障或crash後仍然會存在。通常涉及將Data寫入Stroage，像是Disk。


*Transaction: 指的是一連串相關的操作或task*