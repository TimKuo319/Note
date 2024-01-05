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
+ metadata
	+ type

### Schema based validation

### Object schema validation

### Binding validation pipes

### Class validator

