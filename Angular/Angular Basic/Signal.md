

- [ ] signal 

## Signal

signal 是 angular 16 以後推出的新語法，本質上就是一個裝著值的容器，但相對傳統狀態管理機制是透過 zone.js 在偵測到事件(ex: click、keydown)，或是 input 改變時，讓 zone.js 去掃過所有 component 找到對應的 template 做更新。Signal 會主動去通知 angular 框架來減少這個逐一掃描的行為，增進效能。

signal 相關文件可以參考 [Signals • Overview • Angular](https://angular.dev/guide/signals#writable-signals) 

