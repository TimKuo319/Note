
## SQL Syntax 
### IN

+ 當要檢視某一個值是否符合`某些值的集合`時，可使用`IN`。


```SQL
SELECT * FROM users WHERE userName in ("test1", "test2", "test3")
```

等價於

```SQL
SEL[ECT * FROM users WHERE userName = "test1",  user"test2", "test3")
```

### BETWEEN

+ 用來篩選在某個區間之內的值

```SQL
SELECT <columns> FROM <your_table> WHERE <column> BETWEEN <minimum> AND <maximum>;
```

### LIKE

+ 利用LIKE來進行比較鬆散的配對
+ `%`代表的是任意數的字元(包含0)，所以下方的句子就會找出所有開頭為Harry Botter的條件
+ 依照不同的DB，有的是`case-sensitive`，有的則不是

```SQL
SELECT <columns> FROM <your_table> WHERE <column> LIKE "Harry Botter%"
```


### CREATE TABLE

```SQL
CREATE TABLE <table_name>  
(  
<column_name> <data_type>,  
<column_name> <data_type>,  
<column_name> <data_type>,  
<column_name> <data_type>,  
...  
)
```

### Drop TABLE

刪除table

```SQL
DROP TABLE <table-name>
```

### INSERT INTO

新增資料

1. 提供column name對應的value
```sql
INSERT INTO _table_name_ (_column1_, _column2_, _column3_, ...)  
VALUES (_value1_, _value2_, _value3_, ...);
```

2. 依照table的順序填入值
```sql
INSERT INTO _table_name_  
VALUES (_value1_, _value2_, _value3_, ...);
```

## Data types in SQL

### Numeric 

常見屬性
+ INT
+ INTEGER
+ FLOAT
+ DOUBLE
+ BIGINT
### String 

+ CHAR
	+ 不可變長度字串
	+ 如果宣告一個char(20)，輸入的字串只有10個字元，剩餘的10個字元會被空格填滿，也就是說，無論輸入為多少字元，`占用的空間都是相同的`
+ VARCHAR
	+ 可變長度字串
	+ 假設宣告varchar(255) ，但實際上只使用了10個字元，那就只會佔10個字元的空間
+ BINARY
+ TEXT
### Date and Time

+ DATE
+ DATETIME
+ TIMESTAMP
+ TIME
+ YEAR

### Backup

+ 可以利用`mysqldump`來將資料庫的資料dump出來做備份
	+ `--database`參數代表要備份哪個資料庫
	+ `--table`可以指定備份特定的table
```sh
mysqldump -u"username" -p "database" > <fileName>.sql
```
+ 匯入
	+ 進入mysql client
```sh
source <dumpfilepath>
```


## Join Table

```sql
select * from <tableName> join <tableName> on <column> where <clause>
```

+ Inner join
	+ 只找交集的部分
+ Outer join
	+ Left
		+ 
	+ Right
	+ Full


## Union

```sql
SELECT column1, column2, ... FROM table1 UNION SELECT column1, column2, ... FROM table2;
```

+ 用於合併兩個或多個`select`query的resultset
+ 會移除重複的紀錄

>[!info] Union vs Union All
>+ Union合併重複資料
>+ Union All不合併重複資料

## Intersect
```sql
SELECT column1, column2, ... FROM table1 INTERSECT column1, column2, ... FROM table2;
```

+ 取交集

## Except

```sql
SELECT column1, column2, ... FROM table1 EXCEPT SELECT column1, column2, ... FROM table2;
```
+ 移除右邊`select` query出的結果

### SubQuery

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name operator (SELECT column_name
                            FROM table_name
                            WHERE condition);

```

+ subquery在搭配in做使用的時候只能撈出一個欄位



## Reference

安裝教學: https://medium.com/@jieshiun/%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%9D-mysql-community-server-c87c0b25ad80

資料型態: [MySQL 學習筆記(四) — MySQL中的資料類型 Data Type — 如何創建tables資料表?如何操作資料表? — 快速為自己創建一個資料表 | by Chwang | Medium](https://chwang12341.medium.com/mysql-%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-%E5%9B%9B-mysql%E4%B8%AD%E7%9A%84%E8%B3%87%E6%96%99%E9%A1%9E%E5%9E%8B-data-type-%E5%A6%82%E4%BD%95%E5%89%B5%E5%BB%BAtables%E8%B3%87%E6%96%99%E8%A1%A8-%E5%A6%82%E4%BD%95%E6%93%8D%E4%BD%9C%E8%B3%87%E6%96%99%E8%A1%A8-%E5%BF%AB%E9%80%9F%E7%82%BA%E8%87%AA%E5%B7%B1%E5%89%B5%E5%BB%BA%E4%B8%80%E5%80%8B%E8%B3%87%E6%96%99%E8%A1%A8-927e0c365d6e)