
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
+ `_`表示一個字元
+ 依照不同的DB，有的是`case-sensitive`，有的則不是

```SQL
SELECT <columns> FROM <your_table> WHERE <column> LIKE "Harry Botter%"
```


### CREATE TABLE

```SQL
CREATE TABLE table_name (
    <column1> <datatype> <constraints>,
    <column2> <datatype> <constraints>,
    <column3> <datatype> <constraints>,
    ...,
    PRIMARY KEY (column),
    FOREIGN KEY (column) REFERENCES other_table(column)
);
```

```sql
CREATE TABLE user (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL
);

```

```sql
CREATE TABLE article (
    article_id BIGINT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    content TEXT NOT NULL,
    user_id BIGINT,
    FOREIGN KEY (user_id) REFERENCES user(id)
);

```

## 反引號跟單引號的差別

+ 反引號 
	+ 用來區分table name或column name等，避免與sql keyword產生衝突
+ 單引號
	+ 用於輸入或選擇`值`的時候

### Drop TABLE

刪除table

```SQL
DROP TABLE <table-name>
```


## Join(todo)

### Inner join

### Left join

### Right join

### Cross join

[SQL CROSS JOIN 交叉連接 - SQL 語法教學 Tutorial](https://www.fooish.com/sql/cross-join.html)

## Rand 

+ RAND產生0~1之間的浮點數，FLOOR將值取為整數

```sql
FLOOR(1 + (RAND() * 10))
```

### Group by

group by指令用於將查詢結果中的特定欄位相同值分成群組

```sql
SELECT column_name(s), aggregate_function(column_name) FROM table_name WHERE column_name operator value GROUP BY column_name1, column_name2...;
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

3. 搭配`SELECT`進行批量插入，以及使用`NOT EXISTS`進行檢查，避免新增重複值
```sql
INSERT INTO _table_name_
SELECT VALUE
WHERE NOT EXISTS (SELECT ....)
```

4. 新增遇到重複`primary key`時，可以使用`ON DUPLICATE KEY UPDATE ...`來解決。這樣的做法在遇到重複已經存在的資料時，會選擇用`update`的方式來更新資料，而不會報錯

```sql
INSERT INTO variant (product_id, color_id, size_id, stock)
SELECT p.id, c.id, s.id, 100 -- 假設初始庫存為100
FROM product p
CROSS JOIN color c
CROSS JOIN size s
WHERE c.name IN ('新顏色1', '新顏色2', '新顏色3')
  AND s.size IN ('新尺寸1', '新尺寸2', '新尺寸3')
ON DUPLICATE KEY UPDATE stock = stock;  
```

`ON DUPLICATE KEY UPDATE column = value`的做法是決定要怎進行更新，以上面的例子來說，就是該row的stock會等於該row的stock，也就是說，在遇到重複的時候，就不更新的意思。



### Update

```sql
UPDATE table_name SET column1=value1, column2=value2, column3=value3··· WHERE some_column=some_value;
```

```sql
UPDATE `order`
SET isPaid = 1
WHERE order_id = 123;
```


### Delete

```sql
DELETE FROM table_name WHERE column_name operator value;
```
### Limit Offset

limit、offset通常會放在query的最後，limit表示的是要拿出幾筆資料，offset則表示要從第幾筆資料開始(不包含）。 

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s)
LIMIT number_of_rows OFFSET offset_value;
```

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 3 OFFSET 5;
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


### ENUM

+ 可指定某張表只能有特定的值

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    order_date DATE NOT NULL,
    time ENUM('morning', 'afternoon', 'anytime') NOT NULL
);

```

### Date and Time

+ DATE - YYYY-MM-DD 
+ DATETIME  - YYYY-MM-DD HH:MM:SS
+ TIMESTAMP - 類似於`DATETIME`， 通常用於在資料庫中的建立或修改時間
+ TIME
+ YEAR


### Backup

+ 可以利用`mysqldump`來將資料庫的資料dump出來做備份
	+ `-p` - 指定資料庫
	+ `--table`可以指定備份特定的table
```sh
mysqldump -u"username" -p "database" > <fileName>.sql
```
+ 匯入

1. 入mysql client
2. 建立要匯入的資料庫
```SQL
create <database >
```

3. 使用該資料庫
```SQL
use <database>
```

4. 匯入備份的sql檔案
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

+ 常用於過濾數據，以下面的例子來說，因為只想要撈出特定的departmentID，所以用subquery的方式先過濾數據。
```sql
SELECT * 
FROM Employees 
WHERE DepartmentID IN (SELECT DepartmentID FROM Departments WHERE Location = 'New York');
```


+ subquery在搭配in做使用的時候只能撈出一個欄位

## 建立 Index

```SQL
CREATE IDNEX idx_user_email ON users(email);
```

## 創建和管理用戶授權

+ 創建新使用者
```sql
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';

```

- `%`為萬用字元，將上面的`localhost`換成`%`的話代表使用者可以從任何一個地方連線進來。

+ 查詢資料庫中的所有使用者
```SQL
select user, host from mysql.user;
```

+ 授予用戶權限
```sql
GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' WITH GRANT OPTION;
```

with grant option(?)

## Reference

安裝教學: https://medium.com/@jieshiun/%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%9D-mysql-community-server-c87c0b25ad80

資料型態: [MySQL 學習筆記(四) — MySQL中的資料類型 Data Type — 如何創建tables資料表?如何操作資料表? — 快速為自己創建一個資料表 | by Chwang | Medium](https://chwang12341.medium.com/mysql-%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-%E5%9B%9B-mysql%E4%B8%AD%E7%9A%84%E8%B3%87%E6%96%99%E9%A1%9E%E5%9E%8B-data-type-%E5%A6%82%E4%BD%95%E5%89%B5%E5%BB%BAtables%E8%B3%87%E6%96%99%E8%A1%A8-%E5%A6%82%E4%BD%95%E6%93%8D%E4%BD%9C%E8%B3%87%E6%96%99%E8%A1%A8-%E5%BF%AB%E9%80%9F%E7%82%BA%E8%87%AA%E5%B7%B1%E5%89%B5%E5%BB%BA%E4%B8%80%E5%80%8B%E8%B3%87%E6%96%99%E8%A1%A8-927e0c365d6e)

備份: https://code.yidas.com/mysqldump/


