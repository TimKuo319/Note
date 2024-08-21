
## Normalization

+ 將entity的描述資料，透過一定的程序將table簡化，直到一張表只描述一個單純的事實為止


## Keys

+ Unique keys
	+ 唯一值
	+ 可以是Null
	+ 每個table可以有很多unique key
	+ 可以被`修改為一個新的值`
	
+ Primary keys
	+ `不能是null`
	+ 一張table只能有一個
	+ `不能`被修改

+ Foreign keys
	+ 用來跟其他table做關聯
	+ `Foreign key constrain`
		+ Foreign Key的值`必須是另外一張表中的存在的primary key`
		+ 可以搭配`casacade`在父table中的值有更動時執行對應的操作

```sql
CREATE TABLE table_name (
    <column1> <datatype> <constraints>,
    <column2> <datatype> <constraints>,
    <column3> <datatype> <constraints>,
    ...,
    PRIMARY KEY (column),
    FOREIGN KEY (column) REFERENCES other_table(column)
);
```

### 級聯操作

+ syntax
```sql
Foreign key (column) references other_table(column) on <operation> <action> 
```

+ opertation
	+ `on delete`
		+ 在刪除時執行，
	+ `on update`
		+ 更新時執行

+ action
	+ `casacade`
	+ `set null`
	+ `set default`
	+ `no action`
		+ 即使子table中已經沒有可以參考的foreign key，不會馬上報錯，而是在未來查詢時報錯。
	+ `restrict`
		+ 與`no action`類似，但通常立即檢查`constraint`，而不是在commit時檢查。

## Relationship

+ one-to-one
	+ 資料表之間的row是一對一對應的
	+ 使用者帳密對應到使用者詳細資訊
	+ 用來提升效能
		+ 將frequently access data與infrequently access data分開

+ one-to-many
	+ 最常見的類型
	+ 一個客戶可以有多個訂單，但一筆訂單只能有一個客戶
	+ `foreign key`在many那邊
	
+ many-to-many
	+ 一個學生可以選修很多門課，一門課也可以被很多個學生選修
	+ 會依照兩張table的primary key再額外建一個資料表(`junction table`)儲存這個資訊
		+ 實際上就是變成兩個one-to-many的情況
		+ `foreign key`在`junction table`中

>[!info] one-to-one vs one-to-many
>重點在證明唯一性，以上面的例子來說，訂單這個總體可以有很多個客戶，但因為一筆訂單`只能`對應到一個客戶，所以才會是一對多關係


## Join(todo)

### Inner join

### Left join

### Right join

### Cross join


## Reference

[Day 32 資料庫正規化(一~三) - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10229472)


[SQL CROSS JOIN 交叉連接 - SQL 語法教學 Tutorial](https://www.fooish.com/sql/cross-join.html)