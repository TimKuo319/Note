---
date: 2024-01-10 14:38
---
---
## Guard
Guard這個字有守衛的意思，在Nest中，它的用途主要就是用來決定一個request是否能夠進到request handler，通常就是`驗證`這個步驟。在express中，這個步驟通常會透過middleware來實作，但也因此並不會知道接下來究竟是哪個request hander負責處理這個request。而Nest的guard在這點上透過取得`Execution Context`，可以知道接下來是哪個request handler會負責處理，就像`excpetion filter`、`pipe`這樣可以專門針對某個method透過guard的設計來維持程式碼的簡潔。

### Authorization Guard

```typescript
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    const request = context.switchToHttp().getRequest();
    return validateRequest(request);
  }
}

```

每一個guard都必須要實作`canActivate`這個function，透過回傳`true`以及`false`來決定request是否會被處理。
### Execution context

上面提到的`Exewcution context`是一個繼承自`ArgumentHOst`的class，其中包含一些可以取得請求相關的method跟variable，也多了一些helper function來讓我們使用。
### Role-based authentication

```typescript
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class RolesGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    return true;
  }
}
```

以上程式碼是一個簡單的guard範例，不論請求為何，都會回傳true。
### Binding guards

這邊的作法與前面`pipe`、`exception filter`類似，利用`@UseGuards`來應用guard，全域的話則可以使用`app.useGlobalGuards()`，想要全域使用並且讓他加入DI的話就是在app_module的metadata中加入他。

## Setting roles per handler

