---
tags:
  - Kafka
  - ZooKeeper
---
## What is `ZooKeeper`, why Kafka need it

在 [[What is Kafka]] 中，有提到，Kafka 作為一個分散式系統，利用許多的 broker 來儲存資料增加 throughput，而這些 broker 之間的協調依靠的就是 Zookeeper(在 Kafka 0.9~2.x 版本)。

ZooKeeper 負責的是 broker 之間的協調，具體在 Kafka 中的作用如下

### 1. **Broker 的註冊與 Cluster Membership**

每台 Kafka broker 在啟動時都會註冊到 ZooKeeper，讓其他 broker 和客戶端知道目前有哪些 broker 存活於集群中

### 2. **Controller 選舉**

ZooKeeper 負責選出「controller broker」──負責 partition replica leader 選舉與觸發 failover，確保集群的健康運作

### 3. **Topic / Partition Metadata 管理**

Kafka 將 topic、partition 數量、replica 清單等配置信息儲存在 ZooKeeper 中，確保所有 broker 都有一致的 cluster 視圖

### 4. **Consumer Group 協調（早期版本）**

在 Kafka 0.9 以前，consumer group membership、offset 管理與 rebalance，是透過 ZooKeeper 做協調與存儲的。


## Summary

ZooKeeper 負責 broker 之間的協調，而 Kafka 則負責實際資料的儲存。

但在 Kafka 較新的版本中，已經透過 KRaft 來取代 ZooKeeper 來減少使用 Kafka 的成本。

## Reference

- [What is ZooKeeper & How Does it Support Kafka?](https://dattell.com/data-architecture-blog/what-is-zookeeper-how-does-it-support-kafka/)

- [Using Kafka With ZooKeeper | OpenLogic](https://www.openlogic.com/blog/using-kafka-zookeeper)