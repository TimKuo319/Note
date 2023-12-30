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
>因為middleware如果是在module中應用，他所能應用到的controller或provider範圍僅限於在`@Module()`裝飾器中有註冊的，也因此範圍就是在同一個模組內。當然如果這個middleware的應用範圍是整個application的話，就能夠去引入任意的provider做DI了。