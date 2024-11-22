
## Caculator

透過填寫服務計算預估價格

## ElastiCache

+ 建立
	1. 設計自己快取
	2. 選擇叢集快取 
	3. 節點 -> t2.micro
	4. 下一步
	5. 決定是否需要加密
	6. 建立安全群組
	6. 決定是否需要備份
	7. 決定是否需要log
+ 配置sg
	+ 確保`elasticache`允許來自`ec2`流量
	+ 確保`ec2`也允許來自`elasticache`的流量
	+ 確保兩者在同一個`vpc`下
		+ ec2查看
			+ 選擇ec2 instance
			+ 選網路 -> 看`vpc id`

+ 連線測試
```
redis-cli -tls -h <cache-hostname> -p 6379
```

+ tls參數是在設定時傳輸加密才需要填寫


### Factory設定


```java
public RedisConnectionFactory redisConnectionFactory() {  
    RedisStandaloneConfiguration redisStandaloneConfiguration = new RedisStandaloneConfiguration(server, port);  
    redisStandaloneConfiguration.setPassword(password);  
    LettuceClientConfiguration clientConfig = LettuceClientConfiguration.builder()  
        .commandTimeout(Duration.ofSeconds(3))  
        .shutdownTimeout(Duration.ofMillis(100))  
        .clientOptions(ClientOptions.builder()  
            .sslOptions(SslOptions.builder().build())  
            .disconnectedBehavior(ClientOptions.DisconnectedBehavior.REJECT_COMMANDS)  
            .build())  
        .useSsl()  
        .build();  
  
    return new LettuceConnectionFactory(redisStandaloneConfiguration, clientConfig);  
}
```


## Reference

[ElastiCache Security Options. ElastiCache… | by Jerry’s Notes | What’s next? | Medium](https://medium.com/jerrynotes/elasticache-security-options-c5e11cd06713)

## to learn

sg怎麼設定比較好？

