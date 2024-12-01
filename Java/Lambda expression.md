---
tags:
  - Java
---
Lambda expression 是 Java 8 後引入的一種功能，可以更簡潔的表示匿名函式。以本質上來說，他算是一種 *Functional Programming* 的方式。讓我們可以將 function 作為參數傳遞。

Lambda 的語法很單純，主要就是透過前面的`()`作為參數，後面中括號內部則為 function 的實作。

```java
(parameters) -> { body }
```

Lambda 需要搭配 `FunctionalInterface` 一起使用，`FunctionInterface` 指的是只有一個抽象方法的介面。常見的使用場景像是 [[Comparator and Comparable]] 的 `Comparator` 等。或是在 stream 中的 `filter`、`forEach` 等等。

## Terminology

- `Functional Programming` - 函數程式設計，指的是函數可以做為變數、參數、回傳值等形式存在。

## Reference

[Lambda基础 - Java教程 - 廖雪峰的官方网站](https://liaoxuefeng.com/books/java/functional/basic/index.html)