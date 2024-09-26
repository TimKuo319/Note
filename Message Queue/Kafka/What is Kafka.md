
## 核心概念

- `Producer`

- `Consumer`

- `Broker`

- `Topic`

- `Partition`

	- 一個 topic 可以管理多個 partition，每一個 partition 就是一個 data queue，會依照傳入的順序被 consumer 拿取。

	- 在流量變大的時候，partition 可以被擴展到不同的 broker，應對大流量的架構。
	
- `Offset`

	- 不同 partition 之間的 offset 都是互相獨立的。

	- 每一個訊息的獨立編號，透過查看當前的 offset 就可以知道 partition 被 consumer 消耗的數量。

- `Zookeeper`

## 優勢

- 可以承受高吞吐量

- 能夠提供 realtime 數據傳輸

- 分散式架構

## 常見使用場景

- 活動追蹤系統

- Message queue

- Realtime data 處理

- log aggregation