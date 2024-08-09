
## 使用Redistream


## 概念

+ `stream`
	+ 用來儲存message的地方
+ `consumer group`
	+ 用來接收訊息的group
+ `consumer`
	+ 在讀取消息時給一個consumer名稱就會動態建立使用者
	+ 長時間沒使用後就會被redis刪除

+ 訊息狀態
	+ 已交付
	+ 待處理(pending)
	+ 已確認(ACK)
	+ 未確認
### 常見指令



+ `XADD`
	+ 將資料加入到stream中
```
XADD race:usa 0-* racer Prickett
```
+ `XRANGE key start end`
	+ 尋找start跟end區間的資料，包含頭尾
	+ `+` 代表最小
	+ `-` 代表最大

```
XRANGE mystream - +
```
+ `XREAD`
	+ read data from stream
	+ 最後的數字代表使用從哪個message id開始讀取
```
XREAD COUNT 2 STREAMS race:france 0
```
+ `XGROUP`
	+ used to create、delete 、manage consumer group
	+ `$`: 建立群組後指定要從哪個message id開始進行讀取，`$`表示最後一個消息id，也就是這個group只會接收接下來`新來的message`
	+ `race:france` - 要讀取的stream
	+ `france_riders` - group name
```
XGROUP CREATE race:france france_riders $
```
+ `XREADGROUP`
	+ is used to read from a stream via a consumer group.
	+ 讀取兩類型訊息
		1. 新訊息
		2. 待處理訊息
	+ `>`
		+ ==用來代表只處理最新id以後的id，也就是沒辦法處理之前的id==
	+ 
```
XREADGROUP GROUP <group_name> <consumer_name> 讀取筆數 STREAMS <讀取stream> <id> 
```
```
XREADGROUP GROUP italy_riders Alice COUNT 1 STREAMS race:italy >
```
+ `XINFO GROUPS stream_key`
	+ 用來查詢當前stream的`consumer group`

+ `XINFO CONSUMERS <stream> <groupname>`
	+ 檢查該群組的使用者
+ `XACK`\
	+ allows a consumer to mark a pending message as correctly processed.
## Reference

[Message Queue in Redis - DEV Community](https://dev.to/lazypro/message-queue-in-redis-38dm)