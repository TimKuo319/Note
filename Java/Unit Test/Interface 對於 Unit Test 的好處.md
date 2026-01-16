

interface 在程式設計上能夠用來解耦，降低程式碼之間的相依性，而這點在測試中同樣成立。

在透過單元測試進行 mock 的時候，若是要 mock 的對象是一個具備實作的具體類別，就會需要測試框架花費時間去產生實體類別的物件。除此之外，若是這個實作類別的對象是 final class 或是其中具備 final method 則也無法進行 mock，因此造成單元測試與實作類別綁得太緊。 


以下面程式碼來說，`RealPaymentGateway` 是一個具體的實作類別，同樣可以透過 mock 來模擬。但就會有內容中提到的缺點

```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {
    
    @Mock
    private RealPaymentGateway paymentGateway;  // ⚠️ 問題來了
    
    @InjectMocks
    private OrderService orderService;
    
    @Test
    void shouldPlaceOrderSuccessfully() {
        // 這樣可以 mock，但有幾個問題：
        
        // 1. Mock 具體類別需要 Mockito 做字節碼操作（較慢）
        when(paymentGateway.charge(any()))
            .thenReturn(PaymentResult.success());
        
        // 2. 如果 RealPaymentGateway 是 final class，就 mock 不了
        // 3. 如果有 final 方法，也 mock 不了
        // 4. 測試和實作綁得太緊，重構困難
    }
}
```