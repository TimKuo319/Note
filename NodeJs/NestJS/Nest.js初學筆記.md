---
date: 2023-12-11 Mon 23:11
---
---

## Platfrom

## Controller


## Service


## Two different options nest manipulating responses

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

Query Param of uri

DTO
	class-validator
	class-transformer
Decorator使用
Utility object

app.module可以怎麼寫