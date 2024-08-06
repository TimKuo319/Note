
## Method

用來對四種不同資料結構操作

+ `template.opsForValue`
	+ 用來對simple value(`其實就是redis中的string`)進行操作
+ `templat.opsForHash`
+ `template.opsForList` 
+ `template.opsForSet`

+ `template.expire(key, timeout, timeUnit)`
	+ 設定過期時間

+ `@Cacheable`
	+ 替`spring boot`自動配置cache


## RedisTemplate config

在默認沒有調整的情況下，redisTemplate會使用`jdk serilizer`來處理資料，但因為`jdk serilizer`是直接將資料轉換成二進位格式，不易於我們進行閱讀或除錯，所以通常會需要自己配置`redisTemplate`相關的config，來決定我們要將資料序列化成甚麼樣子。

```java
public class ResiConfig {  
    @Bean  
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {  
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();  
        try {  
            redisTemplate.setKeySerializer(new StringRedisSerializer());  
            redisTemplate.setDefaultSerializer(new GenericJackson2JsonRedisSerializer());  
            redisTemplate.setConnectionFactory(factory);  
        } catch (Exception e) {  
            log.error("Error getting Redis Template connection", e);  
        }  
        return redisTemplate;  
    }  
}
```


以上面的程式碼來說
+ `redisTemplate.setKeySerializer(new StringRedisSerializer())`;
	+ 會將透過java傳出去的值序列化成`String`的格式
	+ 從redis拿回來的值也會被反序列化成`String`
+ `redisTemplate.setDefaultSerializer(new Ger.....)`
	+ redis value將會被序列化為`json`格式
	+ 從redis反序列化回來的值也會變成`json`格式，如果是塞dto給他，回來的值也會自動是dto
	
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
[Effective and highly available cache: Integrating Spring Boot with Redis | by McKinsey Digital | McKinsey Digital Insights | Medium](https://medium.com/digital-mckinsey/effective-and-highly-available-cache-integrating-spring-boot-with-redis-2e21c5cec8bd)


