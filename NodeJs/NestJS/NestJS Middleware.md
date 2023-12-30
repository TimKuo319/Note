---
date: 2023-12-30 Sat 14:04
---
---

## Middleware

在Nest中，middleware的概念其實就跟express中middleware的概念是相同的，都是用來處理請求或回應過程中會用到的一些functoin。像是解析request body的`body-parser` 解析cookie的`cookieParser`、增加觀測性的`logger`等等。

## DI

在Nest中，middleware當然也同樣適用DI來引入其他的provider。最初在文檔中看到以下句子時:
>they are able to **inject dependencies** that are available within the same module

還有點疑惑為什麼是只能對同模組下的provider做DI，但繼續往下看就了解原因了。

>There is no place for middleware in the `@Module()` decorator. Instead, we set them up using the `configure()` method of the module class. Modules that include middleware have to implement the `NestModule` interface. Let's set up the `LoggerMiddleware` at the `AppModule` level.

從這段話裡面就可以發現，在`@Module()`裝飾器中並不像前面的controller、provider一樣，有提供註冊的方式。要將middleware應用到module中的話，需要讓module class去implement `NestModule`這個interface，並透過`configure`來將middleware給應用在module中。

所以為什麼只能引用同模組的provider?
>因為middleware如果是在module中應用，他所能應用到的controller或provider範圍僅限於在`@Module()`裝飾器中有註冊的，也因此範圍就是在同一個模組內。

## Applying middware

在前面DI的部分有稍微提到如何去apply middleware，程式碼上並不困難，但有一些部分需要注意:

1. 實作middlware的class需要implement `NestMiddleware` interface及加上`@Injectable()`供注入
2. 引入middleware的module class需要implement `NestModule` interface

然後就可以像以下程式碼利用`configure()`去應用middleware:
```typescript
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('cats');
  }
}
```

`MiddlewareConsumer`是一個helper class，提供了像是`apply()`、`forRoutes()`function等等做應用，`apply()`內可以填入一個或多個middlware(class或function都可以)，多個的話以逗點隔開。而`forRoutes()`則是指定這些middleware要應用在哪個路徑上。當然可以像以上程式碼一樣指定特定路徑。或是像以下程式碼排除某些方法或路徑
```typescript
consumer
  .apply(LoggerMiddleware)
  .exclude(
    { path: 'cats', method: RequestMethod.GET },
    { path: 'cats', method: RequestMethod.POST },
    'cats/(.*)',
  )
  .forRoutes(CatsController);
```

## Global middleware

要在整個application中使用middleware的話，可以利用像express的`app.use()`方式來應用middleware到全域中。

```typescript
const app = await NestFactory.create(AppModule);
app.use(logger);
await app.listen(3000);
```

像上述的用法，對於global middleware來說，他是沒有法取得DI Container。因此也可以像前面一樣在`AppModule`下去利用`forRoutes('*')`來應用到所有route。


其他更詳細的用法可以參考官方文檔
[Middleware | NestJS - A progressive Node.js framework](https://docs.nestjs.com/middleware)
