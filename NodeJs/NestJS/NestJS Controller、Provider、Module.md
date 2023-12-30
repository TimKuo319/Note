---
date: 2023-12-11 Mon 23:11
update: 2023-12-30 Sat  14:04
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
```typescript
import { Controller, Get, Post, Body } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';
import { CatsService } from './cats.service';
import { Cat } from './interfaces/cat.interface';

@Controller('cats')
export class CatsController {
  constructor(private catsService: CatsService) {}

  @Post()
  async create(@Body() createCatDto: CreateCatDto) {
    this.catsService.create(createCatDto);
  }

  @Get()
  async findAll(): Promise<Cat[]> {
    return this.catsService.findAll();
  }
}
```
從上述程式碼中可以看到建構子中的`private catsService: CatsService`，這個部分就是在做`Dependency Injection`，也因為是透過constructor來進行injection所以被稱作`constructor-based injection`

#### Property-based injection
```typescript
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class HttpService<T> {
  @Inject('HTTP_OPTIONS')
  private readonly httpClient: T;
}
```

關於`Property-based injection`，官方文檔內容提到的使用時機是當上層class也依賴於一個或多個其他provider的時候，如果藉由super()的方式一路進行呼叫則會非常繁瑣，在Nest中就可以使用`Inject()`decorator來解決這個問題。但目前因為還沒有實際利用到這種方式，可能等待日後遇上時才會比較了解。

以上兩種方式都是可行的，但官方建議在沒有特殊情況的狀況下都使用`constructor-based injection`的方式來進行。

## Modules

像是前面提到的`Provider`、`Controller`等等，在Nest中都有透過decorator來標示，而`Module`當然也不例外，他也一樣有`@Module`decorator，而這個decorator通常會包含以下屬性:
+ Providers
	+ 前面提到的，基本上會是跟這個module有相關的service , helper ,factory等等
+ Controllers
	+ 前面提到過的，Controller是用來處理請求、回應、路由等等，而這部分就是放與module有相關的controller
+ Imports
	+ 用來引入其他模組，通常是為了利用那些模組中的provider
+ Exports
	+ 用來導出provider，也因此這個部分的provider會是這個module provider的subset，目的就是為了讓其他模組能夠使用這個模組的provider


### Module re-exporting
```typescript
@Module({
  imports: [CommonModule],
  exports: [CommonModule],
})
export class CoreModule {}
```

當一個模組引入外部模組時，可以再將這個外部模組給導出，以上面的例子來說，當有其他人要import CoreModule的時候，他們也能夠使用到CommonModule的內容。
### Dynamic module

在官方文檔overview中提到較少，未來有實際使用到或再更深入了解時再做筆記。

