
## 使用Redistream

### 常見指令

+ `stream`
	+ 用來儲存message的地方
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
	+ 建立群組後指定要從哪個message id開始進行讀取，`$`表示最後一個消息id，也就是這個group只會接收接下來`新來的message`
	+ `race:france` - 要讀取的stream
	+ `france_riders` - group name
```
XGROUP CREATE race:france france_riders $
```
+ `XREADGROUP`
	+ is used to read from a stream via a consumer group.
+ `XACK`\
	+ allows a consumer to mark a pending message as correctly processed.
## Reference

[Message Queue in Redis - DEV Community](https://dev.to/lazypro/message-queue-in-redis-38dm)