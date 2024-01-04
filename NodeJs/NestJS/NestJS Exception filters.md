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

`@Catch()`decorator內填入exception的類型，他告知Nest這個filter想要catch的exception。而所有的exception filter都需要implement `ExceptionFilter`這個interface，若是有多個exception要catch的話就以`comma-seperated list`的形式傳進decorator中。

### Binding filters

要套用exception filter的方式也很簡單，可以在我們寫好的http method前面透過`@UseFilters`來達成。

```typescript
@Post()
@UseFilters(new HttpExceptionFilter())
async create(@Body() createCatDto: CreateCatDto) {
  throw new ForbiddenException();
}
```

>Prefer applying filters by using classes instead of instances when possible. It reduces **memory usage** since Nest can easily reuse instances of the same class across your entire module.

上述的做法利用在`@UseFilters` 中直接new一個`HttpExceptionFilter`的instance，但官方建議直接傳入class就好。原因是因為，直接傳入class的話，在Nest下會透過DI的方式注入這個filter，如此一來，整個application在其他地方注入`HttpExceptionFilter`的時候，就能會使用同一個instance，可以減少記憶體上的使用。

上面程式碼做法的filter只限於該method的範圍，是method-scope的。要應用到controller scope可以像以下這樣
```typescript
@UseFilters(new HttpExceptionFilter())
export class CatsController {}
```
其實就是在controller前加上`@UseFilters`

至於global scope的應用跟前面middleware的方式有點像。

```typescript
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalFilters(new HttpExceptionFilter());
  await app.listen(3000);
}
bootstrap();
```

利用`app.userGlobalFilters`來全域使用filter。

而這裡也跟一樣middleware一樣，因為是在module外去使用這個filter，所以這個方式是沒辦法讓其他module透過DI的方式注入filter。如果想要讓這個filter可以透過DI的方式注入的話，需要使用另外一種方式。
```typescript
import { Module } from '@nestjs/common';
import { APP_FILTER } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_FILTER,
      useClass: HttpExceptionFilter,
    },
  ],
})
export class AppModule {}

```

透過在`app module`中以`provider`的形式讓Nest自己去初始化這個filter，讓他融入到app中。

### Inheritance

有時候我們只是想要繼承`global exception filter`並加上一些功能來符合我們的需求。這時候可以讓我們定義的filter去繼承`BaseExceptionFilter`這個class，並且呼叫繼承下來的catch method，官方程式碼如下。

```typescript
import { Catch, ArgumentsHost } from '@nestjs/common';
import { BaseExceptionFilter } from '@nestjs/core';

@Catch()
export class AllExceptionsFilter extends BaseExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    super.catch(exception, host);
  }
}
```
## Reference

[Exception filters | NestJS - A progressive Node.js framework](https://docs.nestjs.com/exception-filters)