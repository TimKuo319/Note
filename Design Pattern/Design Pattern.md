
### What is design pattern , why we need it ? 

Design pattern 是一種軟體的設計模式，目的在於解決開發流程中常見的設計問題，這些 pattern 都是前人在不斷開發的過程中總結出來的結果，是目前公認的大部分設計問題的解決方案之一。

### Relationship between SOLID and Design pattern

+ Design pattern 本身是用來解決軟體問題的一種實現，其中大多本身都已經遵守了一些 SOLID 原則

+ SOLID 本身只是一種良好軟體的概念，要搭配 design pattern 這樣的實現來做出一個良好的軟體，兩者是相輔相成的

+ 在實務上的使用場景可能會是，遇到特定的設計問題的時候先想這個問題適合什麼 design pattern ，並透過 SOLID 來驗證我們實作出來的 design pattern ，有沒有違反其中的某些規則，有的話在進行程式碼的修改。

*盡可能的去符合這些規則或模式，但避免過度設計，只要能夠減少重構或修改的成本，都是你有成功套用上面方式的體現*


### Factory 

+ Goal - `使現有存在的 class 不需要進行改變`

+ `Simple Factory`

+ `Factory pattern method`
		+ Factory Interface
			+ 工廠介面
		+ Concrete Factories
			+ `實作`工廠介面的實體
		+ Product Interface
			+ 產品介面 
		+ Concrete Factories
			+ `實作`產品介面的實體
		
+ `Abstract Factory Pattern`
### Strategy 

+ Goal - `處理各種既相同，但做起來又不太一樣的行為，將行為抽離(Strategy Interface)後，視情況隨意組合切換，將行為包裝成一個類別(Concrete Strategy)`

+ `Strategy Interface`
	+ 策略介面 -> 協助我們抽換實作
+ `Concrete Strategy`
	+ 策略實作 -> 我們希望使用到的行為
+ `Context Class`
	+ 引用策略介面的類別
	+ 調用 context class 的物件可以去自行傳入想要的`Conrete Strtegy`

```java
public class OrderProcessor {
    public void processOrder(String paymentMethod) {
        if (paymentMethod.equals("CreditCard")) {
            processCreditCardPayment();
        } else if (paymentMethod.equals("PayPal")) {
            processPayPalPayment();
        } else {
            throw new IllegalArgumentException("Unsupported payment method: " + paymentMethod);
        }
    }

    private void processCreditCardPayment() {
        System.out.println("Processing credit card payment...");

    }

    private void processPayPalPayment() {
        System.out.println("Processing PayPal payment...");

    }
}

public class Main {
    public static void main(String[] args) {
        OrderProcessor processor = new OrderProcessor();
        processor.processOrder("CreditCard"); 
        processor.processOrder("PayPal"); 
    }
}

```
### Reference

+ [Design Pattern | 從復仇者看策略模式（ Strategy Pattern ） feat. TypeScript | by 神Q超人 | Enjoy life enjoy coding | Medium](https://medium.com/enjoy-life-enjoy-coding/design-pattern-%E5%BE%9E%E5%BE%A9%E4%BB%87%E8%80%85%E7%9C%8B%E7%AD%96%E7%95%A5%E6%A8%A1%E5%BC%8F-strategy-pattern-feat-typescript-8623989c5e46)

### to learn

+ [ ] `solid`, `design pattern` , `clean architecture` 關係
