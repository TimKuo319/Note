---
tags:
  - TypeScript
---
## Void

可以用來表示沒有任何沒有返回值的 function

一個 void 變數沒什麼用處，因為 void 變數只能被給定 `null` 或 `undefined`

## Null 和 Undefined 

在 TypeScript 中，可以使用 `null` 和 `undefined` 來定義這兩個原始資料型別：

```ts
let u: undefined = undefined;
let n: null = null;
```

與 `void` 的區別是，`undefined` 和 `null` 是所有型別的子型別。也就是說 `undefined` 型別的變數，可以賦值給 `number` 型別的變數：

```ts
// 這樣不會報錯
let num: number = undefined;

// 這樣也不會報錯
let u: undefined;
let num: number = u;
```


## Reference

- [原始資料型別 | TypeScript 新手指南](https://willh.gitbook.io/typescript-tutorial/basics/primitive-data-types)