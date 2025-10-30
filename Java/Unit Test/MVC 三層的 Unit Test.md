

## Service、Dao 

- `Transactional`

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