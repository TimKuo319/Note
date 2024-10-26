---
tags:
  - lock
  - database
---
## What is `Lock`, why we need it

在多執行序同時存取相同資源的情況下，會因為沒辦法確定執行的先後順序而造成資料不一致，這就是常見的 `Race Condition`。而鎖就是解決他的方式。

## Terminology

## Reference

- [理解資料庫『悲觀鎖』和『樂觀鎖』的觀念. 沒有經過實際操作的理論都只是空談，這篇文章我會附上自己的MySQL 語法… | by 林鼎淵 | Dean Lin | Medium](https://medium.com/dean-lin/%E7%9C%9F%E6%AD%A3%E7%90%86%E8%A7%A3%E8%B3%87%E6%96%99%E5%BA%AB%E7%9A%84%E6%82%B2%E8%A7%80%E9%8E%96-vs-%E6%A8%82%E8%A7%80%E9%8E%96-2cabb858726d)