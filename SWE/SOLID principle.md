---
date: 2024-01-03 Wed 11:00
---
---

## SOLID

SOLID原則是一種軟體設計原則，目標在於讓開發人員設計出容易維護及擴展的軟體。每一個字母分別代表了一個原則。

### Single Responsibility Principle(SRP)

單一職責原則，這個原則指的意思是說一個類別應該只有一個職責。
```java
// 壞的例子
class UserService {
  public void register() {...} 
  public void sendEmail() {...}  
  public void saveToDB() {...}
}

// 好的例子
class UserRegistrationService {
  public void register() {...}
}
    
class EmailService {
  public void sendEmail() {...}   
}

class UserRepository {
  public void saveToDB() {...}
}
```

從上述程式碼可以看到，`UserService`這個類別內包含了相當多的功能，像是註冊、發送email、儲存到資料庫等等。但這樣的做法會造成後續在維護過程的困難，因為對後人來說UserService的東西太雜，需要一個一個去進行確認。但如果是以好的例子來說，各個功能依照需求被區分成