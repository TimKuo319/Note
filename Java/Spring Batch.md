---
tags:
  - Java
  - SpringBoot
---
## What is Spring Batch, why we need it 

>Spring Batch 是一個開源框架，旨在簡化批處理作業的開發，特別是在 Java 環境中。它提供了一組用於處理大量數據的工具和功能，包括數據讀取、處理和寫入。Spring Batch 的目的在於幫助開發人員快速構建可重用的批處理作業，並確保這些作業的可靠性和可擴展性。


## Core concept

- Job - 由一個或多個 step 組成，代表的是一個 batch 處理的過程
- Step - 一個 step 封裝了 ETL 的三個部分，透過 reader、processor、writer，來進行資料的處理，會作為 job 的一部分被執行。

- ItemReader
	- 負責用來將資料從 datasource(db, file) 中讀取出來，會透過 `read` function 將資料傳遞給 itemProcessor 
	- *read function 一次只讀取一筆資料，當讀取完所有資料後一次交給 processor*
- ItermProcessor
	- 讀取來自 reader 的資料，同樣也是透過 `process` 一次處理一筆資料，當處理完所有資料後，一次交給 writer
- ItemWriter
	- 不同於 reader 及 processor，writer 的 `write` 會一次將所有資料寫入目標的資料來源(db, file)，但要注意的部分是，如果要使用批次寫入的方式到 db 的話，可以透過 spring batch 提供的 `JdbcBatchItemWrtier` 讓框架替我們批次寫入。如果是自己使用 `jdbcTemplate` 的話，要記得透過 `batchUpdate` 來避免一筆一筆插入資料，降低效能的行為。


## Reference

- [初學者指南 Spring Batch 的入門與應用 - 卡哥小技倆 Carger Tips](https://carger.tips/%E5%88%9D%E5%AD%B8%E8%80%85%E6%8C%87%E5%8D%97-spring-batch-%E7%9A%84%E5%85%A5%E9%96%80%E8%88%87%E6%87%89%E7%94%A8)
- [Chunk-oriented Processing :: Spring Batch](https://docs.spring.io/spring-batch/reference/step/chunk-oriented-processing.html#page-title)
- [Understanding Spring Batch: A Comprehensive Guide | by Nouhaila El Ouadi | Medium](https://medium.com/@elouadinouhaila566/understanding-spring-batch-a-comprehensive-guide-393904ac401c)