---
tags:
  - is/evergreen/substantiated
---

## I/O Multiplexing and Non-blocking I/O

與 redis 建立的每一個連線會透過 socket 與 redis 進行通訊。而 redis 能夠透過 single thread 達成高併發主要就是依靠上面兩個特性 `I/O Multiplexing` 以及 `Non-blocking I/O` 。

### I/O Multiplexing
`I/O Multiplexing` 是一種透過單個執行緒管理多個 I/O 操作的方式。在 redis 中，它會使用到像是 `select()`、`poll()`、`epoll()` 這類 system call 來去監聽每一個已經註冊好的 socket，一但這些 socket 的資料準備好，kernel 就會通知 redis 去解讀 socket 傳遞的訊息並做後續的動作。

### Non-blocking I/O
`Non-blocking I/O` 則是對於每一個 socket，redis 會將它們設定為 `non-blocking`，這樣的做法會讓 client 在執行一些需要比較長時間的指令或資料分段傳輸時，不會 block 住其他 client 的資料，使得 redis 能夠快速處理各個客戶端傳來的訊息。


## Reference

- [How Redis Stays So Fast, Even with Just One Thread | by Abhinav Singh | Medium](https://medium.com/@abhi.strike/how-redis-stays-so-fast-even-with-just-one-thread-6aa79235ea8f)
## Related Reading

- [淺談I/O Model. 前言 | by Carl | Medium](https://medium.com/@clu1022/%E6%B7%BA%E8%AB%87i-o-model-32da09c619e6)
- [Non-Blocking Vs Blocking I/O | by Tarun Jain | Medium](https://tarunjain07.medium.com/non-blocking-vs-blocking-i-o-notes-904559ae5b9e#0a31)