---
date : 2023-05-14 13:49
alias : []
---

---

## Classical Problems of Synchronization

### Bounded Buffer

### Readers and Writers 

+ Readers : only read the data set ; they do not perform any updates
+ Writers : can both read and write

+ First-Reader-Writer problem : reader不用因為writer在等就需要進入wait狀態
	+ 可能造成writer starvation
+ Seoncd-Reader-Write problem : 如果writer已經準備好，那就應立即開始執行
	+ 可能造成reader starvation

### Dining-Philosophers
