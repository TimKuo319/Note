---
date: 2023-12-11 Mon 23:11
---
---

## Platfrom
Nest本身提供了兩種HTTP platform做為基底，分別是`express`以及`fastify`，預設情況下會以`express`為主。

## Controller

Controller在application中的用途主要是用來處理請求，Routing等等。Controller在官方文檔中篇幅滿長的，如果有學

### Two different options nest manipulating responses

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

### Standard


### Library-specific


## Provider

Query Param of uri

DTO(Data transfer object)(Recommend declare using class instead of type, why)
	class-validator
	class-transformer


Decorator使用
Utility object

app.module可以怎麼寫
send data using built in library