
### What is lock , why lock

當資料庫在同時面對多請求時，可能會出現 [Concurrency problem](<Concurrency problem>) ，所以我們需要加上 lock 來限制讀寫順序，避免同樣的資料被同時修改。

### Type of lock

+ Shared Lock
	+ 允許多個使用者同時對一筆資料進行讀取操作
+ Exclusive Lock
	+ 僅允許使用者對資料
+ Intent Shared Lock

+ Intent Exclusive Lock

+ Update Lock

+ Increment Lock

### Reference

+ https://medium.com/dean-lin/%E7%9C%9F%E6%AD%A3%E7%90%86%E8%A7%A3%E8%B3%87%E6%96%99%E5%BA%AB%E7%9A%84%E6%82%B2%E8%A7%80%E9%8E%96-vs-%E6%A8%82%E8%A7%80%E9%8E%96-2cabb858726d

如何針對 database 進行 lock