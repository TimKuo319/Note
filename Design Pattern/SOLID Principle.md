

## What is Solid,  Why solid

SOLID 是一種 OOP 設計原則， 目的是為了能夠提高軟體的可讀性、擴充性。

這裡指的`類別`是所有 data 與 function的集合，並不限縮於 OOP 中的class

### Single Responsibility Principle

+ `A class should have only one reason to change.`

+ 可以想像成一個模組 (class) 的使用對象 (role) 只會有一個，今天針對這個模組的改動只會影響到一個對象。
	+ 對象可能是其他的 class 又或者是使用者

+ 儘管最終可能導致一個 class 的 function 數量非常少，但只要能維持架構的彈性，這樣的做法是能夠接受的

### Open-Closed Principle

+ `Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification`

+ 軟體在面對擴展時應該是開放的 (易於擴展），且`不應該`修改到原始的程式碼
	+ 擴展指的是易於新增，修改指的是不修改到`原始已經存在的類別`

#### Code Example 

+ 沒有遵守 OCP

```java

// 折扣類型的 enum
public enum DiscountType {
    SEASONAL, 
    FESTIVAL
}

// 計算折扣的類別
public class DiscountCalculator {
    public double calculateDiscount(double price, DiscountType type) {
        switch (type) {
            case SEASONAL:
                return price * 0.10; // 季節性折扣 10%
            case FESTIVAL:
                return price * 0.20; // 節慶折扣 20%
            default:
                return 0;
        }
    }
}

```

```java
public interface Discount {
    double apply(double price);
}

// 季節性折扣實做
public class SeasonalDiscount implements Discount {
    @Override
    public double apply(double price) {
        return price * 0.10; // 季節性折扣 10%
    }
}

// 節慶折扣實做
public class FestivalDiscount implements Discount {
    @Override
    public double apply(double price) {
        return price * 0.20; // 節慶折扣 20%
    }
}


// 如果有新的折扣，只要新增新的實作，不用去變動就有的實作
```



+ *即使功能需要擴充，也不應該修改到原始程式碼*
### Liskov Substitution Principle

+ `If S is a subtype of T, then objects of type T may be replaced with objects of type S without altering any of the desirable properties of the program`

+ 簡單來說，只要Ｓ跟Ｔ替換之後，整個Ｐ的行為沒有差別，那Ｓ就是Ｔ的子型態。從類別上來說，Ｓ完全可以繼承Ｔ成為Ｔ的子類別。

+ 重點在於Ｓ是不是`子型態`，而子型態就是上面定義，在`完全替換`之後，行為`沒有差別`，才能稱為`子型態`，如果是只是單純透過`extend`的方式去繼承一個類別，變成他的子類別，但在替換後出現不一樣的行為，ex: `相同 function 有不同的錯誤等`，則不算是 subtype。

+ 是 subtype 就一定是子類別，但是子類別不是一定是 subtype

#### 7 Rules

+  Covariance of argument
	+ 實作或繼承時，method 的 argument 應該要相同
+  Covariance of result
	+ 實作或繼承時，method的回傳值應該要是`superclass`回傳值對應的`subtype`
+  Exception rule
	+ 當你實作或是繼承一個superClass的方法時 拋出的例外 `必須是superClass方法拋出的例外的subType`

*能夠完全放心相信去使用的多型，就能夠符合以上三點。*

+ Precondition 
	+ subtype 的 precondition 不應該 加強
	
+ Postcondition
	+ subtype 的 postcondition 不應該被削弱

+ Invariant rule
	+ 父型態的 invariants 必須被子型態保留
	+ invariants -> 亙古不變的限制 
		+ ex : 在陣列型態的定義中，capacity 指的就會是陣列內的元素數量，這點不會改變

+ Constraint rule
	+ 隨著時間變化，某些 class property 應該要怎麼改變

>[!info] Constraint vs Invariant
>`Invariant` : 指的是無論何時何地 你單看一個類別的 instance 都應該要遵守的 就叫做invariant
>`Constraint` : 指的是對於某一個物件來說 在歷史上的兩個不同時間 應該要遵循什麼事情


#### Code Example


不符合 LSP 

```java
class Bird{
  public void fly(){
    // Fly!
  }
}
```

```java
class Ostrich extends Bird{
  @Override
  public void fly(){
    // Cannot fly
    throw new RuntimeException();
  }
}
```

```java
Bird b = getBird();
b.fly();
```

### Interface Segregation Principle

+ `No client should be forced to depend on methods it does not use.`

+ 模組與模組之間的依賴，不應該有用不到的功能可以被對方呼叫到

+ 可以透過介面來進行分割，讓 role 只能接觸到應有的功能

### Dependency Inversion Principle

1. `1. High-level modules should not depend on low-level modules. Both should depend on abstractions.`
2. `Abstractions should not depend on details. Details should depend on abstractions.`

+ 高層模組不應依賴低層模組，它們都應依賴於抽象介面。抽象介面不應該依賴於具體實作，具體實作應依賴抽象介面。
	+ 高層模組
		+ 由許多低層模組組合而成的模組 -> MVC 中的 `controller`
	+ 低層模組
		+ 不可分割的原子邏輯層 -> MVC 中的 `model`

+ 在沒有介面的情況下，高層模組會受到低層模組影響，因為實作會限制組合，也就是控制權是在低層模組。但透過介面的做法，可以讓高層模組依賴於抽象層，進而將控制權反轉(提升)到抽象層，高層模組只要知道介面能夠達成的`行為`，即可使用，低層模組則是需要實作介面，`兩者都相依於抽象層`。

+ 我們的 stylish 就是如此，controller 作為最上層的模組，其實是很多 servcie 的組和，如果我們將 service 透過 `interface` 來定義的話，controller 只要知道 service 能替他做到什麼事情就好，不用去管背後的實作內容。


## Reference

+ https://medium.com/%E7%A8%8B%E5%BC%8F%E6%84%9B%E5%A5%BD%E8%80%85/%E4%BD%BF%E4%BA%BA%E7%98%8B%E7%8B%82%E7%9A%84-solid-%E5%8E%9F%E5%89%87-%E7%9B%AE%E9%8C%84-b33fdfc983ca

+ https://www.javaguides.net/2018/02/interface-segregation-principle.html

+ https://www.jyt0532.com/2020/03/22/lsp/

+ [SOLID Design Principles Explained- Stackify](https://stackify.com/solid-design-liskov-substitution-principle/) 

+ [深入淺出 Liskov 替換原則 Liskov Substitution Principle · jyt0532's Blog](https://www.jyt0532.com/2020/03/22/lsp/)

