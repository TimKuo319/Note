---
date: 2023-12-30 Sat 14:04
---
---

## Middleware

在Nest中，middleware的概念其實就跟express中middleware的概念是相同的，都是用來處理請求或回應過程中會用到的一些functoin。像是解析request body的`body-parser` 解析cookie的`cookieParser`、增加觀測性的`logger`等等。而middleware在Nest中當然也可以利用DI的方式注入同模組間相關的dependency。