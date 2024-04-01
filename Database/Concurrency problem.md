---
date: 2024-03-25 22:49 Mon
---
---

## 資料庫的併發問題

指多個事務(transaction)同時執行，對同一筆資料進行讀寫操作時，可能會導致資料不一致或其他異常的問題。

+ Dirty reads
	+ 一個transaction讀取到另一個尚未提交的data，導致讀取到的是dirty data

+ Non-repeatable reads 
	+ 在同一個transaction中，重複讀取同一筆資料，但卻得到不同的值

+ Phantom reads
	+ 在同一個transaction中，連續兩次讀取時，讀取出來的筆數跟上次不同。

+ Lost update
	+ 兩個transaction同時更新一筆data，後面transaction的更新會覆蓋掉前面一筆的更新，造成更新被"遺失"。


## Reference

[資料庫交易的 Isolation. 最近在讀 High Performance MySQL 時讀到了… | by Yuren Ju | getamis](https://blog.amis.com/database-transaction-isolation-a1e448a7736e)[Distributed System: Concurrency Problems in Relational Database | by Bindu C | Medium](https://medium.com/@bindubc/distributed-system-concurrency-problem-in-relational-database-59866069ca7c)
[database - Non-Repeatable Read vs Phantom Read? - Stack Overflow](https://stackoverflow.com/questions/11043712/non-repeatable-read-vs-phantom-read)
