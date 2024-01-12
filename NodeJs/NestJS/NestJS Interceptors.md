---
date: 2024-01-12 Fri 13:20
---
---

## Interceptors

每一個Interceptor都需要implement `NestInterceptor` interface中的`intercept()` method，這個method又包含兩個參數，一個是前面提到過的`ExecutionContext`、以及`CallHandler`。
### Call handler

callhandler參數中有一個`handle()`method是用來呼叫router handler的，如果某個request在被傳遞到request handler的過程中遇到一個沒有call handle的interceptor，那麼這個請求是不會被handler成功執行的。

也就是說interceptor是可以將整個request/response流程風

Observerable

### Binding

### Response mapping

### Stream overriding

return of