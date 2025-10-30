
## JUnit5 常見 annotation

+ `@BeforeAll`
	+ 需為 **static method**
	+ 在執行所有 Testcase 前會執行一次
	
+ `@BeforeEach`
	+ 在跑每一個 Testcase 前都會執行一次
	
+ `@AfterEach`
	+ 在每一個 Testcase 執行完後執行一次
	
+ `@AfterAll`
	+ 需為 **static method**
	+ 在所有 Testcase 執行完後執行一次

- `@Transactional`
	- 如果在 testcase 上方加上 `@Transactional` 註解，則在 testcase 過程中所經歷的 db 更動會在最後被 rollback 回去
	- 相對於在寫 code 的時候使用時 `@Transactional` 則只有在遇到錯誤 (有 exception 時)，才會進行 rollback

- [ ] `@SpringBootTest`

## Shortcut

+ Ctrl+Shift+F10 - `在intellij下執行測試`
## Reference

[Java Test #2: 單元測試- JUnit 單元測試的基礎工具 | by Charlie Lee | Bucketing | Medium](https://medium.com/bucketing/java-test-2-%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6-junit-%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6%E7%9A%84%E5%9F%BA%E7%A4%8E%E5%B7%A5%E5%85%B7-ebf335ff2619)
