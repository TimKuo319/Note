---
tags:
  - Java
  - multi-thread
---
## What is `volatile`, why we need it

`volatile` 是 java 中的一種 keyword，本身的目的在確保所有的 thread 都能夠去看到變數的最新值。

在釐清上面的文字以前，就要先了解一般狀況下 thread 讀取變數的機制。

當一個 thread 在讀取變數的時候，實際上 cpu 幫我們做的事情是到 `main memory` 中去讀取變數，並且將這個變數複製一份副本出來，放在 thread 的 `cpu cache`。透過這樣的方式去增加讀取的效能。當寫入的時候會先更新自己的 `cpu cache`，再某一個時間點再更新回 `main memory`

然而，在 multi-thread 的狀況下，因為每一個 thread 都有自己的 `cpu cache`，當今天某一個 thread 修改了變數，且變數也更新到 `main memory` 時，其他 thread 會因為是讀取自己 `cpu cache` 的原因所以會造成==沒有讀取到最新更新的資料==。

而 `volatile` 在替我們做的事情就是，在變數被更新的時候，直接去更新 `main memory` ，並且無效化所有 thread 的 `cpu cache` 來讓每個 thread 都能讀取到最新的資訊。


## Terminology 

- cpu cache

## Reference

[What role has the VOLATILE keyword in Java?](https://www.youtube.com/watch?v=V2hC-g6FoGc)
