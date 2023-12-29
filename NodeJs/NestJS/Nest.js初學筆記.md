---
date: 2023-12-11 Mon 23:11
---
---

## Platfrom
Nest本身提供了兩種HTTP platform做為基底，分別是`express`以及`fastify`，預設情況下會以`express`為主。

## Controller

Controller在application中的用途主要是用來處理請求，Routing等等。Controller在官方文檔中篇幅滿長的，以下就針對自己覺得較不熟悉的部分作紀錄。

### Two different options nest manipulating responses

在Nest中，一共有兩種不同的處理請求方式，接下來會搭配著以下程式碼介紹。
```typescript
import { Controller, Get } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(): string {
    return 'This action returns all cats';
  }
}

```
#### Standard
以下是官方文件:
>Using this built-in method, when a request handler returns a JavaScript object or array, it will **automatically** be serialized to JSON. When it returns a JavaScript primitive type (e.g., `string`, `number`, `boolean`), however, Nest will send just the value without attempting to serialize it. This makes response handling simple: just return the value, and Nest takes care of the rest.

簡單的說，當今天要回應請求時，利用return回應的值是JavaScript object或是陣列的話，他就會自動將這些值轉換成JSON。如果今天回傳值是原始資料類型，則回直接回傳。像是`status code`、`header`之類的都可以完全交給Nest處理，也是官方比較推薦的方式
#### Library-specific
以下是官方文件:
>We can use the library-specific (e.g., Express) [response object](https://expressjs.com/en/api.html#res), which can be injected using the `@Res()` decorator in the method handler signature (e.g., `findAll(@Res() response)`). With this approach, you have the ability to use the native response handling methods exposed by that object. For example, with Express, you can construct responses using code like `response.status(200).send()`.

`Library-specific`的做法就會比較自由一點，可以在response時做更多自定義的處理。而在Nest中觸發`Library-specific`的方式就是直接去呼叫native處理請求的物件。像是以下程式碼，透過`@Req decorator`來引用express的request object
```ts
import { Controller, Get } from '@nestjs/common';
import { Request } from 'express';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(@Req() request: Request): string {
    return "this is a response"
  }
}
```

## Provider

service、repositorys、factories，helpers等等這些都可以視為provider，可以想像成是許多不同類型的服務用來供controller使用，將這些各自切開以提升程式碼的獨立性、簡潔性。而這其中有很重要的一點，*Provider可以作為dependency被注入!*

### Dependency Injection(DI)
參考 :
1. [淺入淺出 Dependency Injection. DI 有什麼好？如何實作 DI？DI 到底是什麼？ | by Wenchin | Wenchin Rolls Around | Medium](https://medium.com/wenchin-rolls-around/%E6%B7%BA%E5%85%A5%E6%B7%BA%E5%87%BA-dependency-injection-ea672ba033ca)
2. [Angular - Understanding dependency injection](https://angular.io/guide/dependency-injection)


### Ways to do injection

#### Constructor-based injection
```

```

#### Property-based injection


Query Param of uri

DTO(Data transfer object)(Recommend declare using class instead of type, why)
	class-validator
	class-transformer


Decorator使用
Utility object

app.module可以怎麼寫

inversion of control 
