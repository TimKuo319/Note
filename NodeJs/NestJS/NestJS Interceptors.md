---
date: 2024-01-12 Fri 13:20
---
---

## Interceptors

每一個Interceptor都需要implement `NestInterceptor` interface中的`intercept()` method，這個method又包含兩個參數，一個是前面提到過的`ExecutionContext`、以及`CallHandler`。
### Call handler

callhandler參數中有一個`handle()`method是用來呼叫router handler的，如果某個request在被傳遞到request handler的過程中遇到一個沒有call handle的interceptor，那麼這個請求是不會被handler成功執行的。

也就是說interceptor是可以將整個request/response流程封裝(?)起來的。可以在request處理的前/後實作其他功能邏輯。在請求被處理前實作其他邏輯的方式是在呼叫`handle()`前進行處理，至於請求被處理完的話，則是透過`Rxjs`內的`observable`來進行處理

### Response mapping

### Stream overriding

return of