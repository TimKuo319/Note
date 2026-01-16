
## Mockito

- 可搭配 beforeEach 做使用

- [ ] `@MockBean` vs `SpyBean`
	- MockBean 產生出的是一個完全假的 bean
	- SpyBean 產生出的是半真半假的 bean，只有被替換過的 method 會是自定義的其他都與原先的 bean 相同。



	- `@Mock` 模擬出一個完全獨立的假物件
	- `@MockBean` 模擬出一個假的物件用於抽換 IoC container 中的內容
		- 實際場景 -> 當進行整合測試時，如果 service 實際上依賴了其他物件，這時如果透過 `@Autowired` 注入 service，其中被依賴的物件也會被丟到 IoC container 中，如果想要抽換這些 bean，就可以透過 `@MockBean` 來抽換

```java

// 外部依賴的真實實作
@Service
public class StripePaymentGateway implements PaymentGateway {
    private final RestTemplate restTemplate;
    private final String apiKey;
    
    @Override
    public PaymentResult charge(BigDecimal amount) {
        // 真的會呼叫 Stripe API！
        String url = "https://api.stripe.com/v1/charges";
        // ... HTTP 請求
        return parseStripeResponse(response);
    }
}

@Service
public class SendGridEmailService implements EmailService {
    private final SendGridClient sendGridClient;
    
    @Override
    public void sendConfirmation(String email) {
        // 真的會發送郵件！
        Email mail = new Email();
        mail.setTo(email);
        sendGridClient.send(mail);
    }
}

// 業務邏輯層
@Service
public class OrderService {
    private final OrderRepository orderRepository;
    private final PaymentGateway paymentGateway;  // Spring 會注入真實實作
    private final EmailService emailService;      // Spring 會注入真實實作
    
    public OrderService(OrderRepository orderRepository,
                       PaymentGateway paymentGateway,
                       EmailService emailService) {
        this.orderRepository = orderRepository;
        this.paymentGateway = paymentGateway;
        this.emailService = emailService;
    }
    
    @Transactional
    public OrderResult createOrder(Order order) {
        // 1. 儲存到資料庫（要測試這個！）
        Order saved = orderRepository.save(order);
        
        // 2. 呼叫支付（不想真的扣款！）
        PaymentResult payment = paymentGateway.charge(order.getAmount());
        
        if (!payment.isSuccess()) {
            throw new PaymentException("Payment failed");
        }
        
        // 3. 發送郵件（不想真的發信！）
        emailService.sendConfirmation(order.getCustomerEmail());
        
        return OrderResult.success(saved.getId());
    }
}


```


```java
@SpringBootTest
@AutoConfigureMockMvc
class OrderIntegrationTest_WithMockBean {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Autowired
    private OrderService orderService;  // 真實的 Service
    
    @Autowired
    private OrderRepository orderRepository;  // 真實的 Repository（測試資料庫）
    
    // ⭐ 用 @MockBean 替換外部依賴
    @MockBean
    private PaymentGateway paymentGateway;  // 替換 StripePaymentGateway
    
    @MockBean
    private EmailService emailService;  // 替換 SendGridEmailService
    
    @Test
    @Transactional  // 測試後自動 rollback
    void shouldCreateOrderSuccessfully() throws Exception {
        // Arrange - 控制 mock 的行為
        when(paymentGateway.charge(any(BigDecimal.class)))
            .thenReturn(PaymentResult.success("mock-txn-123"));
        
        String orderJson = """
            {
                "customerId": "CUST-001",
                "customerEmail": "test@example.com",
                "amount": 100.00
            }
            """;
        
        // Act - 測試完整的 API 流程
        MvcResult result = mockMvc.perform(post("/api/orders")
                .contentType(MediaType.APPLICATION_JSON)
                .content(orderJson))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.orderId").exists())
            .andReturn();
        
        // Assert - 驗證各層的行為
        
        // 1. 驗證資料庫操作（真實的！）
        List<Order> orders = orderRepository.findAll();
        assertThat(orders).hasSize(1);
        assertThat(orders.get(0).getCustomerId()).isEqualTo("CUST-001");
        assertThat(orders.get(0).getAmount()).isEqualByComparingTo("100.00");
        
        // 2. 驗證支付服務被正確呼叫（mock！）
        verify(paymentGateway).charge(new BigDecimal("100.00"));
        
        // 3. 驗證郵件服務被正確呼叫（mock！）
        verify(emailService).sendConfirmation("test@example.com");
        
        // 優點：
        // ✅ 沒有真的呼叫 Stripe
        // ✅ 沒有真的發送郵件
        // ✅ 但完整測試了 Controller → Service → Repository 的流程
        // ✅ 測試快速、穩定、可重複執行
    }
    
    @Test
    @Transactional
    void shouldRollbackWhenPaymentFails() throws Exception {
        // Arrange - 模擬支付失敗
        when(paymentGateway.charge(any()))
            .thenReturn(PaymentResult.failed("Card declined"));
        
        String orderJson = """
            {
                "customerId": "CUST-001",
                "amount": 100.00
            }
            """;
        
        // Act
        mockMvc.perform(post("/api/orders")
                .contentType(MediaType.APPLICATION_JSON)
                .content(orderJson))
            .andExpect(status().isBadRequest());
        
        // Assert
        // 1. 驗證資料庫沒有髒資料（@Transactional 回滾）
        assertThat(orderRepository.count()).isEqualTo(0);
        
        // 2. 驗證不該發送郵件
        verify(emailService, never()).sendConfirmation(anyString());
        
        // 這個測試證明了：
        // ✅ @Transactional 正確運作
        // ✅ 支付失敗時不會建立訂單
        // ✅ 不會發送確認信
    }
}
```



- 不可 mock 
	- static method
	- private method
	- final class


## Reference

[Java Unit Test — Mockito. 在軟體工程上測試是一項重要的工作，軟體測試基本流程可分為單元測試 (Unit… | by Vivi Wang | Medium](https://vivifish.medium.com/java-%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6%E5%B7%A5%E5%85%B7-mockito-e5f0ce93579d)