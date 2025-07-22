---
tags:
  - SpringSecurity
  - SpringBoot
---

透過 Spring Security 進行 RBAC 的用法並不複雜。只要針對需要進行權限限制的 endpoint 使用 `.hasRole()` method 就能夠針對特定的 api 進行權限限制。

那我們要怎麼拿到使用者的權限呢？

通常在 DB 中會有 user 的資料與他對應的權限，所以我們只要去將 DB 的這些資訊撈出來與 Spring Security 結合，就能夠讓 Security 去判斷是否符合權限。

這時就會需要去實作 `UserDetailsService`，從 [[Spring Security Note By Gemini]] 中可以知道 `UserDetailsService` 負責的是取得使用者資料的部分，這個 interface 也只包含一個 function `loadUserByName`，只要在這個 function 中取得權限並轉換成 Security 可以讀取的形式 `GrantedAuthority` 就可以讓 security 判斷該使用者是否有對應的權限。

## Code example

```java
//SecurityConfig.java

package org.example.config;  
  
import jakarta.servlet.FilterChain;  
import jakarta.servlet.ServletException;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.security.config.annotation.web.builders.HttpSecurity;  
  
import org.springframework.security.core.userdetails.User;  
import org.springframework.security.core.userdetails.UserDetails;  
import org.springframework.security.core.userdetails.UserDetailsService;  
import org.springframework.security.provisioning.InMemoryUserDetailsManager;  
import org.springframework.security.web.SecurityFilterChain;  
import org.springframework.web.filter.OncePerRequestFilter;  
  
import java.io.IOException;  
import java.util.Set;  
import java.util.HashSet;  
  
@Configuration  
public class SecurityConfig {  
    @Autowired  
    private UserDetailsService userDetailsService;  
  
    @Bean  
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {  
        http  
            .authorizeHttpRequests(auth -> auth  
                .requestMatchers("/public").permitAll()  
                .requestMatchers("/admin").hasRole("ADMIN")  
                .requestMatchers("/user", "/user/json").hasRole("USER")  
                .anyRequest().authenticated()  
            )  
            .formLogin(form -> form  
                .defaultSuccessUrl("/hello", true)  
            )  
            .userDetailsService(userDetailsService)  
            .addFilterBefore(new JsonContentTypeFilter(), org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter.class);  
        return http.build();  
    }  
  
}
```


```java
// CustomUserDetailsService.java

package org.example.config;  
  
import org.example.entity.User;  
import org.example.entity.Role;  
import org.example.repository.UserRepository;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.security.core.GrantedAuthority;  
import org.springframework.security.core.authority.SimpleGrantedAuthority;  
import org.springframework.security.core.userdetails.UserDetails;  
import org.springframework.security.core.userdetails.UserDetailsService;  
import org.springframework.security.core.userdetails.UsernameNotFoundException;  
import org.springframework.stereotype.Service;  
  
import java.util.Set;  
import java.util.stream.Collectors;  
  
@Service  
public class CustomUserDetailsService implements UserDetailsService {  
    @Autowired  
    private UserRepository userRepository;  
  
    @Override  
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {  
        User user = userRepository.findByUsername(username)  
                .orElseThrow(() -> new UsernameNotFoundException("User not found: " + username));  
        Set<GrantedAuthority> authorities = user.getRoles().stream()  
                .map(role -> new SimpleGrantedAuthority("ROLE_" + role.getName()))  
                .collect(Collectors.toSet());  
        return new org.springframework.security.core.userdetails.User(  
                user.getUsername(),  
                user.getPassword(),  
                authorities  
        );  
    }  
}
```


