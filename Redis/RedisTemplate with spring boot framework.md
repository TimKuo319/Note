
## Method

+ `template.opsForValue` 
	+ 用來對simple value(`其實就是redis中的string`)進行操作
	+ `set`
		+ 用來對新增key-value pair到redis中


+ `@Cacheable`
	+ 替`spring boot`自動配置cache


## RedisTemplate config

在默認沒有調整的情況下，redisTemplate會使用`jdk serilizer`來處理資料，但因為`jdk serilizer`是直接將資料轉換成二進位格式，不易於我們進行閱讀或除錯，所以通常會需要自己配置`redisTemplate`相關的config，來決定我們要將資料序列化成甚麼樣子。


## RedisConnectionFactory

+ spring data redis中的一個接口，用來創建和管理與redis server的connection
+ 提供connection poll management
+ 處理connection的獲取與釋放

## 序列化與反序列化

+ 序列化
	+ 通常指將資料變成一個可用的傳輸格式
		+ ex : js中，將object變為json格式傳遞
+ 反序列化
	+ 將資料從通用的傳輸格式轉換回程式語言可用的狀態
		+ ex: js中，接收到json格式後將他轉換回object以利後續使用

## Reference

[Java and Redis](https://redis.io/learn/develop/java/getting-started)
[Introduction to Spring Data Redis | Baeldung](https://www.baeldung.com/spring-data-redis-tutorial)