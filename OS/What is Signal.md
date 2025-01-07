---
tags:
  - OS
---

## What is `Signal`, why we need it

signal 是作業系統中用來通知 process 發生特定事件的機制。他的來源可以來自 OS、其他 process、甚至是他自己本身，當 process 收到 signal 後，如果沒有特別設定 signal handler 的話，就會依照 signal 預設的處理行為。

### Signal 來源

- Kernel event : 非法的記憶體訪問、除零錯誤 (`SIGFPE`) 
- Externel event : 使用者輸入 `CTRL+C` (觸發`SIGINT`) 、`CTRL+Z`(觸發`SIGSTP`)
- 其他 process 

關於各種 Signal 可以透過以下指令來查詢
```sh
man signal
```


## Reference

- [Signals in Operating Systems:. A Silent Language of Communication | by José Fernández | Medium](https://medium.com/@4990/signals-in-operating-systems-7c7b90523b61) 

- [探索 Linux 訊號與後台進程管理：安排定期啟動腳本 | 運行時啟動腳本 - DevTech Ascendancy Hub](https://devtechascendancy.com/linux-signal-process-management/#google_vignette)