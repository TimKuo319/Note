
一般的 spring security config 可能如下。

```java

@EnableWebSecurity  
public class SecurityConfig {  
  
    @Bean  
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {  
        http  
            .cors(Customizer.withDefaults())  
            .csrf(AbstractHttpConfigurer::disable)  
            .sessionManagement(manager -> manager.sessionCreationPolicy(SessionCreationPolicy.STATELESS))  
            .authorizeHttpRequests((requests) -> requests  
                .anyRequest().authenticated())  
            .addFilterBefore(new GoogleAccessTokenFilter(), UsernamePasswordAuthenticationFilter.class);  
  
        return http.build();  
    }  
}
```

在 security 中配置 cors 的方式有兩種，第一種是利用 `CorsConfigurationSource` 去定義所有有關 cors 的設定。可以參考官方文件 [CORS :: Spring Security](https://docs.spring.io/spring-security/reference/servlet/integrations/cors.html) 。而另外一種方式就是上面使用的方式 `Customizer.withDefaults()` 。

`Customizer.withDefaults()` 會讓 security 去使用默認的 cors 策略，而當專案中有主動去配置 `WebMvcConfigurer` 時，security 就會使用其中的 cors 設定。

下面的 webconfigurer 與上面的 security 在同一個專案下，所以 security 使用 default 時，就會使用下方的配置。

```java
@Configuration  
public class WebConfig implements WebMvcConfigurer {  
  
    @Value("${cors.allow.origins}")  
    private String allowedOrigins;  
  
    @Override  
    public void addCorsMappings(CorsRegistry registry) {  
        registry.addMapping("/**")  
  
            .allowedOrigins(allowedOrigins) // Vue Server URL  
            .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")  
            .allowedHeaders("*")  
            .allowCredentials(true);  
    }  
}
```

