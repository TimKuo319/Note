---
tags:
  - is/evergreen/substantiated
---
## What is `Semaphore`, why we need it

semaphore 是一種用來在 multi-thread / process 下，避免 race condition 的方式。

主要的方式是針對要取用的資源加上一個變數，這個變數的值代表的是**可同時存取此資源的最大數量** 而 process 會透過 `wait()` 及 `signal()` 兩個原子操作的方式來 **獲得 / 釋放** semaphore


- `wait()` - process 要存取資源時，去檢查 semaphore 是否 > 0，否的話代表當前資源已達最大存取數量，block 住該 process，等待其他 process 釋放資源，反之則將 semaphore - 1，並存取資源

- `signal()` - process 釋放資源時，將 semaphore + 1。

## Types of Semaphore

- `Binary Semaphore` -  其實就是排他鎖，只允許一個資源同時最多被一個 process 存取，最大值為 1

- `Counting Semaphore` - 可以允許同時多 process 存取的控制方式，semaphore 最大值大於 1

## Reference

- [Semaphores in Process Synchronization - GeeksforGeeks](https://www.geeksforgeeks.org/operating-systems/semaphores-in-process-synchronization/)