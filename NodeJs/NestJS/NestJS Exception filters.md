---
date: 2024-01-03 Wed 23:00
---
---
## Exception filters

`Exception filter`是Nest中內建的機制，主要是透過`global exception filter`來處理
一些`unhandled filter`來回應user-friendly的response。

### Standard exceptions

在拋出標準例外的情況下，這個`global exception filter`會去處理`HttpException`type以及他子類別的exception，並且會產生一個固定的格式回傳給用戶端。

```json
{
  "statusCode": 500,
  "message": "Internal server error"
}
```

而拋出標準例外的方式如下
```ts
@Get()
async findAll(){
	throw new Exception('Forbidden', HttpStatus.FORBIDDEN);
}

```

詳細的參數說明可以參考官方文檔

### Custom exceptions

在Nest中，我們可能會有需要自己建立客製化的exception來處理，這時候可以透過繼承`HttpException`來讓前面提到的`global exception filter`去接收我們客製的exception，讓Nest來幫我們處理response的部分。

```typescript
export class ForbiddenException extends HttpException {
  constructor() {
    super('Forbidden', HttpStatus.FORBIDDEN);
  }
}
```

### Built-in HTTP exceptions

Nest中還有許多繼承`HttpException`的子類別，可以從官方文檔中看到。而這些exception也都可以藉由`options`這個parameter提供`error cause`以及`error description`。程式碼如下

```typescript
throw new BadRequestException('Something bad happened', { cause: new Error(), description: 'Some error description' })
```
### Exception filters

前面提到，`global exception filter`會去處理`HttpException`以及他的子類別。像前面的exception一樣，exception filter當然也可以自己客製。官方範例如下

```typescript
import { ExceptionFilter, Catch, ArgumentsHost, HttpException } from '@nestjs/common';
import { Request, Response } from 'express';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();
    const status = exception.getStatus();

    response
      .status(status)
      .json({
        statusCode: status,
        timestamp: new Date().toISOString(),
        path: request.url,
      });
  }
}
```

`@Catch()`decorator內填入要catch的exception類型。而所有的exception filter都需要implement `ExceptionFilter`這個interface

### Binding filters

>Prefer applying filters by using classes instead of instances when possible. It reduces **memory usage** since Nest can easily reuse instances of the same class across your entire module.

scope
## Reference

[Exception filters | NestJS - A progressive Node.js framework](https://docs.nestjs.com/exception-filters)