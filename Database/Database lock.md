
### What is lock , why lock

當資料庫在同時面對多請求時，可能會出現 [Concurrency problem](<Concurrency problem>) ，所以我們需要加上 lock 來限制讀寫順序，ex : 