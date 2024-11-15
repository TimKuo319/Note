
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


## JdbcTemplate 的回傳值

jdbcTemplate 有提供兩種回傳值的方式，一種是透過泛型的方式，事先定義好物件的樣子，再將從 database 撈出來的資料 map 成物件，以方便後續使用。具體例子如下，透過將撈出來的資料 map 成一個 user 的 instance 來去做到後續使用。 

```java
public User getUserById(int id) {
    String sql = "SELECT * FROM users WHERE id = :id";
    Map<String, Object> params = Map.of("id", id);

    return namedParameterJdbcTemplate.queryForObject(sql, params, (rs, rowNum) -> {
        User user = new User();
        user.setId(rs.getInt("id"));
        user.setName(rs.getString("name"));
        user.setAge(rs.getInt("age"));
        return user;
    });
}
```

第二種則是透過直接回傳 `Map` 的方式，等到 service 層再去依照用途拿出對應的屬性。以下面的例子來說，回傳值會是 `List<Map<String, Object>>`，其中 String 代表的是 rowName，Object 則代表實際的值，拿出來的結果就會像下面的格式一樣。這樣做的好處在於，當今天查詢的欄位不是 `*` 時，對於已經建立好的 class 來說，可能會有些屬性沒有被填上，導致後續參考的時候可能會意外參照到 null 的屬性。如果是直接回傳 map 到 service 層的話，就能夠在 service 層決定怎麼取用這些值。

```java
[
    { "id" : 1, "name" : "Alice", "age" : 25 },
    { "id" : 2, "name" : "Bob", "age" : 30 },
    { "id" : 3, "name" : "Charlie", "age" : 35 }
]
```

```java
public List<Map<String, Object>> getUsersByAge(int minAge) {
    String sql = "SELECT id, name, age FROM users WHERE age >= :minAge";
    Map<String, Object> params = Map.of("minAge", minAge);

    // 執行查詢並回傳結果
    return namedParameterJdbcTemplate.queryForList(sql, params);
}
```


## Reference

[Getting Started | Accessing Relational Data using JDBC with Spring](https://spring.io/guides/gs/relational-data-access)

[NamedParameterJdbcTemplate (Spring Framework 6.2.0 API)](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/jdbc/core/namedparam/NamedParameterJdbcTemplate.html)

[[Day14] – Spring Boot 與MySQL數據庫的應用教學 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10217667?sc=rss.iron)
