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
```****
class
instance with parameter
ParseUUIDPipe version

### Custom pipes

transfrom method
value 
metadata
	type
	 metatype
		data

### Schema based validation

### Object schema validation

### Binding validation pipes

### Class validator

