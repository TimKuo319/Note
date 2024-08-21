
### What is transaction, why we need it

Transaction 是一種滿足 [ACID原則](<What is transaction and ACID>) 的行為，能夠確保資料一致性與完整性。


### Scope 

Transaction 可以針對單一 query 進行隔離層級，也能夠直接在 db 設定 global 的隔離層級，所有的 transaction 都會套用該層級。 


針對單一 transaction 進行隔離層級設定
```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
SELECT * FROM table_name WHERE id = 1234;
```

針對此次連線進行隔離層級設定
```sql
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```



設定 transaction 時，不指定 isolation level 預設會是什麼 

