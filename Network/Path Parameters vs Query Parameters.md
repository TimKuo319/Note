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


以 API 設計來說，資源的表示通常會使用 `kebab-case` 用於針對資源本身，而 `query paramerter` 則會使用 camelCase 作為命名。

會將 `query parameter` 用 camelCase 的原因是，`query parameter` 本身會是給程式閱讀的，ex : `json`、程式中的變數等等。使用 camelCase 更易於直接在程式中使用。

*註：kebab-case 指的是中間加上分隔線的表示法，如 transaction-logs*

## Reference

- [Path vs. Query Parameters: Choosing the Right Approach for API Requests - DEV Community](https://dev.to/farhatsharifh/path-vs-query-parameters-choosing-the-right-approach-for-api-requests-2lah)