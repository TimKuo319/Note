
## What is Spring Event, Why we need it ?

Spring Event 是 Spring 框架提供基於 pub / sub 的處理機制。 可以讓應用程式內的不同的元件去發出、監聽事件來達到解耦的目的。

透過 Spring Event，除了能夠讓我們不用在 service 層撰寫大量複雜的邏輯，讓 service 層能專注在業務邏輯上，同時達到統一處理事件的效果。 

## 核心元件

### ApplicationEventPublisher

用來發布事件，使用方式也很簡單，在要使用的地方注入 `ApplicationEventPublisher`，並透過 `publishEvent` 去發出事件即可。

```java
@Service
public class UserService {
    
    private final UserRepository userRepository;
    private final ApplicationEventPublisher eventPublisher;
    
    public UserService(UserRepository userRepository, 
                       ApplicationEventPublisher eventPublisher) {
        this.userRepository = userRepository;
        this.eventPublisher = eventPublisher;
    }
    
    public User registerUser(UserRegistrationDto dto) {
        // 1. 執行業務邏輯
        User user = new User();
        user.setUsername(dto.getUsername());
        user.setEmail(dto.getEmail());
        user = userRepository.save(user);
        
        // 2. 發布事件
        eventPublisher.publishEvent(
            new UserRegisteredEvent(this, user.getUsername(), user.getEmail())
        );
        
        return user;
    }
}
```


### ApplicationListener

用來接收事件，在接受到事件後進行處理

```java
@Component
public class UserEventListener {
    
    private final EmailService emailService;
    private final NotificationService notificationService;
    
    public UserEventListener(EmailService emailService, 
                            NotificationService notificationService) {
        this.emailService = emailService;
        this.notificationService = notificationService;
    }
    
    // 同步監聽（預設）
    @EventListener
    public void handleUserRegistered(UserRegisteredEvent event) {
        log.info("User registered: {}", event.getUsername());
        emailService.sendWelcomeEmail(event.getEmail());
    }
    
    // 異步監聽
    @Async
    @EventListener
    public void sendNotification(UserRegisteredEvent event) {
        notificationService.sendWelcomeNotification(event.getUsername());
    }
    
    // 條件監聽
    @EventListener(condition = "#event.amount > 1000")
    public void handleLargeOrder(OrderCreatedEvent event) {
        log.info("Large order created: {}", event.getOrderId());
    }
}
```
### ApplicationEvent

定義要用來傳遞的事件，會被對應的 event listener 給接收

```java
// 方式一：繼承 ApplicationEvent（傳統方式）
public class UserRegisteredEvent extends ApplicationEvent {
    private final String username;
    private final String email;
    
    public UserRegisteredEvent(Object source, String username, String email) {
        super(source);
        this.username = username;
        this.email = email;
    }
    
    public String getUsername() { return username; }
    public String getEmail() { return email; }
}

// 方式二：直接使用 POJO（推薦，Spring 4.2+）
public class OrderCreatedEvent {
    private final Long orderId;
    private final BigDecimal amount;
    private final LocalDateTime createdAt;
    
    public OrderCreatedEvent(Long orderId, BigDecimal amount) {
        this.orderId = orderId;
        this.amount = amount;
        this.createdAt = LocalDateTime.now();
    }
    
    // getters...
}
```


## 進階用法

- listener control flow
- conditional listener  
- async listener
- transactional event 

## Reference

[Understanding Spring Boot Events | Medium](https://medium.com/@AlexanderObregon/understanding-the-mechanics-of-spring-boots-event-system-4295597bcca5)

 [秒懂SpringBoot之Spring Events全解析_springboot event-CSDN博客](https://blog.csdn.net/ShuSheng0007/article/details/135860790)

 [Spring Events | Baeldung](https://www.baeldung.com/spring-events)
 