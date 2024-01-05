---
date: 2024-01-05 Fri 10:15
---
---

## Pipes

pipe通常有兩種用途`transformation`，`validation`，為的就是在資料被傳遞到request handler之前進行驗證或資料型態轉換，來使得執行結果能夠符合我們預期。

### Built-in pipes
+ ValidationPipe
+ ParseIntPipe
+ ParseBoolPipe
+ ParseArrayPipe
+ ParseUUIDPipe
+ ParseEnumPipe
+ DefaultValuePipe
+ ParseFilePipe

### Binding pipes
```typescript
@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
  return this.catsService.findOne(id);
}
```

binding pipe的方式可以藉由在param、body、query等decorator後方加上pipe，依照以上的程式碼來說，就是從param中取的名稱為id的參數，並透過`ParseIntPipe`來確保被傳遞到function中的`id`是`number`type，如果不符合，則會拋出例外
```json
{
  "statusCode": 400,
  "message": "Validation failed (numeric string is expected)",
  "error": "Bad Request"
}
```

而上面pipe在decorator中是透過class的方式傳入，讓Nest以DI的方式去做初始化，如果是想要改變一些pipe的特性，則可以像以下程式碼，利用new的方式建立一個instance並搭配不同的參數去使用。

```typescript
@Get(':id')
async findOne(
  @Param('id', new ParseIntPipe({ errorHttpStatusCode: HttpStatus.NOT_ACCEPTABLE }))
  id: number,
) {
  return this.catsService.findOne(id);
}
```

ParseUUIDPipe version

### Custom pipes

```typescript
import { PipeTransform, Injectable, ArgumentMetadata } from '@nestjs/common';

@Injectable()
export class ValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    return value;
  }
}
```

若是想要自己custom pipe，則pipe需要去implement `PipeTransform<T,R>`這個介面`T`指的是`被傳入值的type`而`R`則是指`目標type`，透過實作`transfrom`method 來客製化需要的pipe。

`transfrom` method需要兩個參數。

+ value 
	+ 被傳入pipe的值

+ metadata的型態為`ArgumentMetadata`，其中又包含三個屬性。
	+ type         -> 表示是`@Body()`、`@Query()`、`@Param()`或custom的parameter
	+ metatype -> 提供argument的metatype, ex `String`, 如果沒有定義的話就會被視為`undefined`
	+ data         -> string pased to the decorator, ex `@Body('test')`，data就會是test字串。
### Object schema validation

可以利用`Zod` library來進行schema驗證。另外一種驗證schema的方式則是利用後面會提到的class validator。Binding zod pipe的方式如以下程式碼

```typescript
@Post()
@UsePipes(new ZodValidationPipe(createCatSchema))
async create(@Body() createCatDto: CreateCatDto) {
  this.catsService.create(createCatDto);
}
```

藉由傳入透過`zod`創建的schema到`ZodValidationPipe`，再加上`@UsePipes`來讓`create()`能夠使用到這個Pipe
### Class validator

在利用這個方式之前，要先安裝`class-validator`以及`class-transformer`。
```sh
npm i --save class-validator class-transformer
```

這個做法的好處在於，我們可以在原先定義的object上透過加上一些decorator就能達成驗證的效果，而不需要再額外創造另外一個validation class來負責驗證。

```typescript
import { IsString, IsInt } from 'class-validator';

export class CreateCatDto {
  @IsString()
  name: string;

  @IsInt()
  age: number;

  @IsString()
  breed: string;
}
```

接著就是binding ValidationPipe()，來讓他幫我們檢查內容是否有誤。

```typescript
@Post()
async create(
  @Body(new ValidationPipe()) createCatDto: CreateCatDto,
) {
  this.catsService.create(createCatDto);
}
```

### Global scoped pipes

要將pipes應用到全域的話，可以使用`userGlobalPipes`。
```typescript
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
bootstrap();
```

或註冊到模組下
```typescript
import { Module } from '@nestjs/common';
import { APP_PIPE } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_PIPE,
      useClass: ValidationPipe,
    },
  ],
})
export class AppModule {}
```

### Transformation

因為我們從network中接收到請求內容時，收到的會是一個JSON物件，但在被request handler使用時，這些內容可能會被轉換成`number`、`boolean`，`string`等等，而這個時候就可以透過`parse*`這些內建的pipe來做轉換。像是以下的例子，透過`ParseIntPipe`來將我們從請求中接收到的id轉換為int的型態。

```typescript
@Get(':id')
async findOne(@Param('id', new ParseIntPipe()) id) {
  return this.catsService.findOne(id);
}
```


依照官方文檔，另外一個很有用的case是向以下這樣。

```typescript
@Get(':id')
findOne(@Param('id', UserByIdPipe) userEntity: UserEntity) {
  return userEntity;
}
```

在接收param的時候，在搭配上自定義的`UserByIdPipe`，讓這個pipe去資料庫中撈取這個id的使用者資料，再將他回傳到request handler內作為參數使用。

### Providing defaults

前面提到的`Parse*`類的pipe對於`null`或`undefined`的值是會直接拋出exception的，為了預防他們直接拋出例外，我們可以讓parameter在被 特定pipe(`ParseIntPipe`、`ParseBooleanPipe`)轉換以前，加入`DefaultValuePipe`，來讓預防空值的情況發生。

```typescript
@Get()
async findAll(
  @Query('activeOnly', new DefaultValuePipe(false), ParseBoolPipe) activeOnly: boolean,
  @Query('page', new DefaultValuePipe(0), ParseIntPipe) page: number,
) {
  return this.catsService.findAll({ activeOnly, page });
}
```
