---
tags:
  - Kafka
  - Message_Queue
---
## What is `Kafka`, why we need it

Kafka 是一個分散式的串流平台，所謂的“串流“指的就是**即時的資料流處理**。最常被當作 `message queue`。允許 `producer` 將訊息發送到 `topic` 中，再由 `consumer` 去拿出訊息。常見的使用場境是拿來收集各種 `Metrics`、`Log`等

## Terminology

### 1. **Topics**

主題名稱，`producer` 會將訊息發送到 `topic` 中，而 `consumer` 再去 `topic` 中將訊息給拿出來

### 2. **Partition** 

一個 Topics 會被切分為多個 Partition。Partition 掌握著一個 Topic 的部分數據。
### 3. **Producer and Consumer** 

在系統中分別指的是訊息的產生者及接收者。

### 4. **Brokers** 

系統中獨立處理訊息的單元，會透過 **ZooKeeper** 去管理 cluster。

## Reference

- [Apache Kafka 介紹. Data really powers everything that we… | by Chi-Hsuan Huang | Medium](https://medium.com/@chihsuan/introduction-to-apache-kafka-1cae693aa85e)
- [Day26 - Kafka 介紹 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10337970)
- [細說 Kafka Partition 分區 - 閱坊](https://www.readfog.com/a/1635090175644241920)