---
tags:
  - Kafka
  - Message_Queue
---
## What is `Kafka`, why we need it

Kafka 是一個分散式的串流平台。常被當作 `message queue` 或 `stream processing`。所謂的“串流“指的就是**即時的資料流處理**允許 `producer` 將訊息發送到 `topic` 中，再由 `consumer` 去拿出訊息。常見的使用場境是拿來收集各種 `Metrics`、`Log`等。

Kafka 透過 Pub / Sub 機制來做到 decoupling，讓 producer 跟 consumer 彼此不用直接互相依賴，producer 只要負責持續發送資料，而 consumer 則負責接收資料。透過這樣的方式來增加系統的彈性。

## How Kafka works

Kafka 的 topic 類似於檔案系統中資料夾的概念(取自官方文件說法)，同一類型的資料通常會被放到同一個 topic 中。而 topic 可以被`分成多個 partition 儲存在不同的 Kafka broker 中`，來增加 data 的 throughput。

而在同一個 partition 中，Kafka 確保了資料來源的順序性。使得 consumer 在讀取同一個 partition 時，能夠確定讀到的資料順序與輸入時一致。

一個完整的 consumer 讀取流程如下

### 1. 初始化與設定 (Initialization & Configuration)

- **配置參數**：設定 `bootstrap.servers` (Broker 位址), `group.id` (消費者群組 ID), `auto.offset.reset` (offset重設策略), `enable.auto.commit` (自動提交offset開關), `key.deserializer`, `value.deserializer` 等。
    
- **建立實例**：應用程式透過這些配置建立 `KafkaConsumer` 物件。
    
### 2. 連接與元資料發現 (Connecting & Metadata Discovery)

- **連接 Broker**：消費者連接到配置的 Kafka Broker。
    
- **獲取元資料**：從 Broker 獲取 cluster、topic、partition 及其領導者 (Leader) 等資訊，並定期更新。

*Leader: 每一個 partition 會有屬於自己的 leader broker，只有當遇到錯誤情況才會去啟用有備用資料的 follower broker*
### 3. 加入群組與分區分配 (Join Group & Partition Assignment)

- **發現 group coordinator**：消費者找到管理其 `group.id` 的協調器 Broker。
    
- **加入群組 (Join Group)**：消費者向 group coordinator 發送請求加入群組。
    
- **觸發重平衡 (Rebalance)**：當群組成員變化（加入/離開/崩潰）時，協調器會觸發重平衡。
    
- **分區分配**：群組中的一個消費者被選為領導者（重平衡期間），根據分配策略決定每個消費者負責哪些分區。結果由協調器通知所有成員。
    
- **接收分區**：每個消費者接收其被分配的分區列表，並只從這些分區讀取訊息。
    

### 4. offset管理 (Offset Management)

- **獲取上次提交 offset ：消費者向群組協調器查詢其上次為每個分區提交的最新offset。
    
- **從 offset 開始消費**：從上次提交的 offset 開始讀取訊息。若無已提交 offset，則根據 `auto.offset.reset` 策略決定起始 offset。
    

### 5. 拉取與處理訊息 (Polling & Processing Messages)

- **持續拉取 (Poll)**：消費者持續調用 `consumer.poll(timeout)` 方法。
    
    - 向分配的分區的**領導者 Broker** 發送拉取請求，獲取新的**訊息批次** (`ConsumerRecords`)。
        
    - 若無新訊息，阻塞等待。
        
- **應用程式處理**：遍歷收到的訊息，執行業務邏輯。每條訊息包含主題、分區、offset、鍵、值、時間戳等。
    

### 6. 提交offset (Committing Offsets)

處理完訊息後，消費者需將其處理進度（offset）通知 Kafka：

- **自動提交 (`enable.auto.commit=true`)**：定期（`auto.commit.interval.ms`）自動提交當前處理的 offset。可能導致「At most once」或「At least once」。
    
- **手動提交 (`consumer.commitSync()` / `consumer.commitAsync()`)**：推薦方式，在業務邏輯成功處理後手動提交，可實現更精確的「Exactly once」控制。
    

### 7. 處理重平衡 (Handling Rebalances by Group Coordinator)

- **重平衡監聽器 (`ConsumerRebalanceListener`)**：可註冊回呼函數，在分區被撤銷 (`onPartitionsRevoked()`) 時進行最後的 offset 提交，或在分區被分配 (`onPartitionsAssigned()`) 後初始化狀態。
    
- **心跳機制**：群組協調器監控消費者心跳。超時則認為消費者離線，觸發重平衡。
    

### 8. 消費者關閉 (Consumer Shutdown)

- **關閉**：調用 `consumer.close()` 方法。
    
- **最終提交**：在關閉前進行最後一次offset提交。
    
- **離開群組**：向協調器發送「LeaveGroup」請求，正式退出群組，觸發重平衡。


## Terminology

### 1. Topics

主題名稱，`producer` 會將訊息發送到 `topic` 中，而 `consumer` 再去 `topic` 中將訊息給拿出來

### 2. Partition 

一個 Topics 會被切分為多個 Partition。Partition 掌握著一個 Topic 的部分數據。透過將 Partition 分散在不同的 broker 中來達到高併發的效果。
### 3. Producer and Consumer 

在系統中分別指的是訊息的產生者及接收者。

### 4. Brokers 

系統中獨立處理訊息的單元，會透過 **ZooKeeper** 去管理 cluster。


## Real World Use Cases

- [How Netflix Uses Kafka for Distributed Streaming](https://www.confluent.io/blog/how-kafka-is-used-by-netflix/)

## Reference

- [Apache Kafka 介紹. Data really powers everything that we… | by Chi-Hsuan Huang | Medium](https://medium.com/@chihsuan/introduction-to-apache-kafka-1cae693aa85e)
- [Day26 - Kafka 介紹 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10337970)
- [細說 Kafka Partition 分區 - 閱坊](https://www.readfog.com/a/1635090175644241920)
- [Apache Kafka](https://kafka.apache.org/intro)