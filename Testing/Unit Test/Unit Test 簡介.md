---
tags:
  - UnitTest
---
## What is Unit Test, why we need it

用來驗證程式中最小的可測試單元是否能正常運作，通常針對某個 `method` 或 `function` 進行測試。重點在於 ==隔離==，即每個測試應該只測試一個單元，避免測試結果被外部因素（如資料庫、API）影響。常見的解決方法是使用 ==Mock== 物件來替代這些外部依賴，模擬它們的行為以確保測試專注於單一單元的功能。可以起到以下作用

- 保護程式碼 
	- 具備 ==良好 Testcase== 的測試，在遇到程式碼變更時，會因為程式碼改動，可能造成測試跑不過的狀況，可以讓開發者提早意識到程式碼可能的潛在問題
- 程式碼品質
	- 耦合度低的程式碼會因為 `function` 或 `method` 本身功能單一，所以在撰寫單元測試上相對容易許多，如果發現測試很難寫的時候，就可能是程式碼寫得不好的一種警訊

## Terminology
### 1. **Test Case**

一個具體的測試場景，用來檢查某個功能是否按預期工作。每個測試用例會定義輸入、執行的操作、以及期望的輸出結果。

### 2. **Test Suite**

一組相關的測試用例，通常用來針對特定的功能模塊或類別進行全面測試。測試套件將這些測試用例組織起來，以便一起執行。

### 3. **Test Fixture**

指在執行測試之前所需的設置與準備工作。這通常包括初始化測試所需的數據、配置環境或建立必要的依賴，並且在測試結束後進行清理。JUnit 中常見的方法有 `@Before`, `@After` 來管理測試前後的工作。

### 4. **Assertion**

斷言是用來檢查測試結果是否符合預期的一種語句。常見的斷言包括 `assertEquals()`（檢查兩個值是否相等）、`assertTrue()`（檢查條件是否為真）等。


## Reference

- [Java Test #2: 單元測試- JUnit 單元測試的基礎工具 | by Charlie Lee | Bucketing | Medium](https://medium.com/bucketing/java-test-2-%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6-junit-%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6%E7%9A%84%E5%9F%BA%E7%A4%8E%E5%B7%A5%E5%85%B7-ebf335ff2619)

- [Things I’ve learned from writing a lot of unit tests | by bytedev | Medium](https://bytedev.medium.com/things-ive-learned-from-writing-a-lot-of-unit-tests-2d234d0cfccf)
