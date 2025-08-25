---
tags:
  - Test
  - UnitTest
---

## What is `Test Double`, why we need it

單元測試的目的是為了確保最小單位的程式碼輸入輸出結果符合預期，所以我們的目的會注重在`測試目標(一個 function or method) 本身。

但我們在撰寫程式時，常常會需要跟其他元件互動(其他物件、DB、第三方 API)，若是在做單元測試的過程中真的去使用到這些外部物件，就讓我們的測試變因變多了，因為增加了測試的相依性。

而這也是這篇的主題 **Test Double**。

**Test Double** 直接翻譯為測試替身，其實就是設計假的外部元件來避免測試階段與真實環境做互動。**Test Double** 主要有以下幾種

## stub

- **用途**：提供**固定的回傳值**，用來替代真實依賴，讓被測試的程式可以順利執行。
    
- **重點**：Stub 不關心「有沒有被呼叫、呼叫幾次、參數是什麼」，它只管「如果被呼叫，就回傳預設好的值」。
    
- **場景**：
    
    - 需要資料來源，但不想連真實資料庫。
        
    - 只想要一個「假的輸入」來讓測試跑起來。

```java
class UserRepositoryStub implements UserRepository {
    @Override
    public User findById(int id) {
        return new User("Alice"); // 固定回傳 Alice
    }
}

@Test
void testGetUserName() {
    UserRepository repo = new UserRepositoryStub();
    UserService service = new UserService(repo);

    assertEquals("Alice", service.getUserName(1));
}

```
## mock

- **用途**：除了能回傳值外，Mock 主要用來**驗證交互行為 (interaction)**。
    
- **重點**：Mock 可以檢查「某個方法有沒有被呼叫過？呼叫了幾次？帶了什麼參數？」
    
- **場景**：
    
    - 想驗證 **被測試的物件是否正確呼叫依賴**。
        
    - 例如：Service 有沒有在正確的時機呼叫 Repository。

```java
@Test
void testSaveUser() {
    UserRepository repo = mock(UserRepository.class);
    UserService service = new UserService(repo);

    service.saveUser("Bob");

    // 驗證 repo.save() 是否被呼叫一次，且參數是 "Bob"
    verify(repo, times(1)).save("Bob");
}

```
## fake

- **用途**：提供一個「可以真的運作」但**簡化過的實作**，通常比真正的依賴要輕量。
    
- **特性**：
    
    - 有邏輯，但不是真正的完整系統。
        
    - 常用來避免測試時需要昂貴或複雜的資源。
        
- **場景**：
    
    - **In-Memory Database**：測試時不用連真實 DB，寫一個存在記憶體的假資料庫。
        
    - **Fake Payment Gateway**：不去打第三方 API，而是做個簡單的程式回傳「交易成功」。

```java
class FakeUserRepository implements UserRepository {
    private Map<Integer, User> data = new HashMap<>();

    @Override
    public User findById(int id) {
        return data.get(id);
    }

    @Override
    public void save(User user) {
        data.put(user.getId(), user);
    }
}

```

## spy

- **用途**：監聽（記錄）方法呼叫，允許在測試後**驗證方法是否被呼叫過、呼叫次數與參數**。
- **特性**：
    
    - 跟 Mock 很像，但 Spy 通常是「包裝真實物件」。
        
    - 可以同時有**真實邏輯**與**驗證呼叫行為**。
        
- **場景**：
    
    - 想保留部分真實邏輯，但同時追蹤呼叫狀況。
        
    - 例如：你想測試 `EmailService.send()` 真的被呼叫，但不用真的發 email。

```java
@Test
void testSpy() {
    List<String> list = new ArrayList<>();
    List<String> spyList = spy(list);

    spyList.add("Hello");
    spyList.add("World");

    verify(spyList).add("Hello"); // 驗證呼叫過
    verify(spyList).add("World");

    assertEquals(2, spyList.size()); // 保留真實邏輯
}

```


## mock vs spy

在上面的部分有提到，mock 及 spy 存在一個共同的目的是**驗證呼叫行爲**，兩者都是透過像是 mock()、spy() 去做出提身，那實際上的差異在哪裡呢？

mock 的話比較像是做出一個空殼，讓他呼叫的時候，可以確保程式的互動狀況是正常的。所以其實如果在撰寫程式碼的過程中有做抽象化，**mock 的時候可以直接去 mock interface，就不用去實際 mock 後面的 implementation。**

而 spy 則是會去包裝一個真實物件，像是在這個物件上加上監聽器，去監聽這個物件**實際使用上的變化**，**會執行程式碼的邏輯**。

## Summary

- **Stub** → 提供假輸出
    
- **Mock** → 驗證交互
    
- **Fake** → 簡化的真實實作
    
- **Spy** → 真實物件 + 驗證交互

## Reference

- [單元測試之 mock/stub/spy/fake ? 傻傻搞不清楚 | by CraftsmanHenry | Medium](https://medium.com/@henry-chou/%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6%E4%B9%8B-mock-stub-spy-fake-%E5%82%BB%E5%82%BB%E6%90%9E%E4%B8%8D%E6%B8%85%E6%A5%9A-ba3dc4e86d86)

- [[Day 22] 談 test double 的五種類型 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10273603?sc=rss.iron)  
