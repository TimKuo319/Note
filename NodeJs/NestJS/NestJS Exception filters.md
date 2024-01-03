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



## Reference

[Exception filters | NestJS - A progressive Node.js framework](https://docs.nestjs.com/exception-filters)