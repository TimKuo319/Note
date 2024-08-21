
### 配置多個 config

```java
package org.example.systemdesignsharding;  
  
import lombok.extern.log4j.Log4j2;  
import org.apache.logging.log4j.LogManager;  
import org.apache.logging.log4j.Logger;  
import org.springframework.beans.factory.annotation.Qualifier;  
import org.springframework.beans.factory.annotation.Value;  
import org.springframework.boot.context.properties.ConfigurationProperties;  
import org.springframework.boot.jdbc.DataSourceBuilder;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.jdbc.core.JdbcTemplate;  
  
import javax.sql.DataSource;  
  
@Configuration  
@Log4j2  
public class DataSourceConfig {  
  
  
    @Bean  
    public DataSource shard1(@Value("${shard1.url}") String url  
        , @Value("${shard.username}") String username  
        , @Value("${shard.password}") String password  
        , @Value("${shard.driver-class-name}") String driverClassName) {  
  
        return DataSourceBuilder.create()  
            .url(url)  
            .username(username)  
            .password(password)  
            .driverClassName(driverClassName)  
            .build();  
    }  
  
    @Bean  
    public DataSource shard2(@Value("${shard2.url}") String url  
        , @Value("${shard.username}") String username  
        , @Value("${shard.password}") String password  
        , @Value("${shard.driver-class-name}") String driverClassName) {  
  
        return DataSourceBuilder.create()  
            .url(url)  
            .username(username)  
            .password(password)  
            .driverClassName(driverClassName)  
            .build();  
    }  
  
    @Bean  
    public DataSource shard3(@Value("${shard3.url}") String url  
        , @Value("${shard.username}") String username  
        , @Value("${shard.password}") String password  
        , @Value("${shard.driver-class-name}") String driverClassName) {  
  
        return DataSourceBuilder.create()  
            .url(url)  
            .username(username)  
            .password(password)  
            .driverClassName(driverClassName)  
            .build();  
    }  
  
    @Bean  
    public JdbcTemplate jdbcTemplate1(@Qualifier("shard1") DataSource dataSource1) {  
        return new JdbcTemplate(dataSource1);  
    }  
  
    @Bean  
    public JdbcTemplate jdbcTemplate2(@Qualifier("shard2") DataSource dataSource2) {  
        return new JdbcTemplate(dataSource2);  
    }  
  
    @Bean  
    public JdbcTemplate jdbcTemplate3(@Qualifier("shard3") DataSource dataSource3) {  
        return new JdbcTemplate(dataSource3);  
    }  
  
  
}
```


+ 先將環境變數配成 datasource ，在讓 template 去使用該 datasource

