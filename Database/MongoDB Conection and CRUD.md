---
date: 2023-12-22 Fri 00:12
---
---

利用js搭配nodejs作為伺服器端時，可以利用mongoose這個套件來建立與mongoDB的連線。

```js
const mongoose = require('mongoose')
```

接著開始進行連線
```
mongoose.connect()
```


建立collection schema
利用model() function替schema生成對應的model constructor function
在新增資料時就新增一個該類型的model
利用save() function將資料存進資料庫中對應的collection名稱


undone

資料庫設計
collection關聯
objectid
"資料中的__v參數"