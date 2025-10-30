
## Dao

- [ ] 什麼是 h2，有兼容 postgres 嗎？
- [ ] spring boot UT 的參數設定(在 `/resource` 下有哪些可調整使用設定)
- [ ] 為何 postgres 在 java 中不需要設定 dirver?

- 可透過 h2 資料庫協助模擬進行 dao 層測試
	- 在 `pom.xml` 中加入 h2 dependency
	- test 資料夾下新增 `/resource/application.properties` 來讓 UT 吃到測試用的變數
	- 在 resource 下新增 `schema.sql` 輸入建立 table 語法
		- 建議加上 `IF NOT EXISTS` 語法
	- 在 resource 下新增 `data.sql` 新增資料

## Service 

- `Transactional`
- 可透過 mockito 去模擬 dao bean 來避免外部的依賴

## Controller

在 controller 主要測試的部分是針對 controller 接收 api 的部分有沒有問題，而這個時候就可以透過 MockMvc 來執行模擬的 api call

### MockMvc 常見物件與 annotation

- `@AutoConfigureMockMvc`
	- 用來模擬真實的 api call

- `RequestBuilder`
	- 用來進行要發出請求的細項設定
		- 方法
		- 參數
		- header
		- .....

- `jsonPath`
	- 用來協助驗證 controller 回傳的格式是否正確

```java

mockMvc.perform(requestBuilder)
	.andExpect(jsonPath("$.id", equalTo("Jonh"));
```

- [ ] `andDo`

- `andExpect`
	- 類似 `assert..`

## Reference