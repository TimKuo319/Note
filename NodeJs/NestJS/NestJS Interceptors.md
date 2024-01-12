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

利用interceptor也能夠拿來做request mapping，將requeset handler回傳的資料map到特定到其他資料格式上。以下是官方例子
```typescript
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

export interface Response<T> {
  data: T;
}

@Injectable()
export class TransformInterceptor<T> implements NestInterceptor<T, Response<T>> {
  intercept(context: ExecutionContext, next: CallHandler): Observable<Response<T>> {
    return next.handle().pipe(map(data => ({ data })));
  }
}
```

### Stream overriding

這邊官方文件的做法是簡單展示一個cache interceptor，這個interceptor會直接回傳一個空陣列，而非跑進去request handler。用來示範一個簡單的cache。

```typescript
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable, of } from 'rxjs';

@Injectable()
export class CacheInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const isCached = true;
    if (isCached) {
      return of([]);
    }
    return next.handle();
  }
}
```

*註*