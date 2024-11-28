---
tags:
  - Network
  - Web
---

## Path Parameters vs Query Parameters

`path parameters` 及 `query parameter` 都是常見用來作為參數傳遞的方式。 

`path parameter` 的表示方式主要跟他的名稱一樣，就像是作為路徑放在 URL 中

```sh
/users/{userId}
```

`query parameter` 的表示方式則是以`?`開頭，如果有多個參數則以 `&` 連接，參數是以 key value pair 的形式傳遞。

```sh
/users?name=John&age=30
```

## Usage

在使用場景上，`path parameter` 就像路徑指定資源一樣，一般通常是表示資源的必要部分，像是 `category`、`userId` 等等 

`query parameter` 的話主要是應用在過濾、排序、篩選等場景上。

## Reference

- [Path vs. Query Parameters: Choosing the Right Approach for API Requests - DEV Community](https://dev.to/farhatsharifh/path-vs-query-parameters-choosing-the-right-approach-for-api-requests-2lah)