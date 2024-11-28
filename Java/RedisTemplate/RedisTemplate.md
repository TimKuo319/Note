---
tags:
  - Redis
---
在 Java 中，我們可以透過 `spring-boot-starter-data-redis` 這個套件來幫助我們進行 redis 的操作，而其實這個套件已經大大的簡化了使用上的流程，最基礎的使用方式基本上只要定義好在 pom.xml 定義好 host、port、password 等以及在 redisTemplate 中定義好 serializer 就可以成功了。


```java
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
```

但其實這背後是基於 `spring-boot-starter-data-redis` 已經幫我們進行了一些設定。以一般配置來說，通常要配置 `RedisConnecitonFactory` 及 `RedisTemplate`。

`RedisConnectionFactory` 主要是負責與 redis 連線的相關設定，他是一個 interface，目前主流的實作(Redis client) 有兩個，一個是 `LettceConnectionFactory`，另一個則是 `JedisConnectionFactory`。如果是使用 `spring-boot-starter-data-redis` 的話，預設會是使用 `lettuceFactory`

而 `RedisTemplate` 本身則像是工具一樣，主要是負責 redis 的資料存取等操作。

透過自定義工廠，就可以有想要對應的連線設定，再搭配上負責操作的 redisTemplate，就可以讓我們在 spring boot 中操作 redis。
