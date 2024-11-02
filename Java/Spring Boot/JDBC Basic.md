
JDBC是一種標準的API，用於在Java應用程式中與`關聯式資料庫進行連線`。面對不同類型的資料庫也能夠統一使用JDBC來進行連線，針對不同類型的資料庫只要安裝對應的`Driver`即可。

## Configuration

```
spring.datasource.url=jdbc:mysql://localhost:3306/assignment?serverTimezone=UTC&useSSL=false  
spring.datasource.username=root  
spring.datasource.password=guo123  
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```


+ `url`
	+ jdbc - 表示使用的是 jdbc API
	+ mysql - 使用的資料庫名稱
	+ localhost:3306 - 指定database的位置及port
	+ assignment - database name

	+ 其他參數
		+ serverTimezone
		+ userSSL
+ username
	+ 使用者名稱
+ password
	+ 密碼
+ driver
	+ 依照使用的資料庫尋找對應的driver。

## JDBCTemplate


+ update
	+ 用來執行單一SQL語句
+ batchUpdate
	+ 用來執行多句SQL語句，像是批量的Insert等等
## DAO vs Repository

+ DAO
	+ 進行更底層的`abstraction`、通常需要手動定義各種方法(CRUD)
	+ 適合查詢需要高度靈活的狀況，query複雜的狀況
+ Repository
	+ 較高層次的`abstraction`、透過調用套件的method，來進行一些較通用的查詢
	+ 更高的簡潔性以及便利性


## Rowmapper 

以下方例子來說，query中，sql後方的參數就會是rowmapper，rowmapper會將去遍歷rs(`ResultSet`)，並`透過rs的get method去取得每個row的資料`。
```java
public List<User> testDatabase() {  
    String sql = "select * from user";  
    return jdbcTemplate.query(sql, (rs, rowNum) -> new User(  
            rs.getInt("id"),  
            rs.getString("email"),  
            rs.getString("password")  
    ));  
}
```

## Reference

[Getting Started | Accessing Relational Data using JDBC with Spring](https://spring.io/guides/gs/relational-data-access)

[[Day14] – Spring Boot 與MySQL數據庫的應用教學 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10217667?sc=rss.iron)
