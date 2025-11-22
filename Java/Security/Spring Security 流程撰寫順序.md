
```mermaid
graph TB
    subgraph Developer[開發者需要撰寫的 Bean]
        SC[SecurityConfig<br/>@Configuration]
        UDS[CustomUserDetailsService<br/>@Service<br/>implements UserDetailsService]
        PE[PasswordEncoder<br/>@Bean]
        
        SC -->|@Bean| SFC[SecurityFilterChain]
        SC -->|@EnableMethodSecurity| MS[啟用 @PreAuthorize]
        UDS -->|override| LBU[loadUserByUsername方法]
        LBU -->|return| UD[UserDetails物件<br/>含 authorities]
    end
    
    subgraph AutoConfig[Spring Security 自動配置]
        AM[AuthenticationManager<br/>ProviderManager<br/>自動創建]
        DAP[DaoAuthenticationProvider<br/>自動創建]
        FC[完整的 Filter Chain<br/>自動創建]
    end
    
    subgraph AutoWiring[自動注入關係]
        DAP -.->|自動注入| UDS
        DAP -.->|自動注入| PE
        AM -.->|自動註冊| DAP
        FC -.->|使用| AM
    end
    
    SFC -.->|配置| FC
    
    style Developer fill:#e1f5ff
    style AutoConfig fill:#ffe1e1
    style AutoWiring fill:#fff4e1
```
