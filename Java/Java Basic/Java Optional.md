
## Optional

Optional是用來檢查null的一種語法，可以方便我們減少透過if else來檢查空值。

## Usage

#### 1. 使用 `Optional.of()`

`Optional.of()` 用於創建一個包含非 `null` 值的 `Optional` 對象。如果傳入的值為 `null`，會拋出 `NullPointerException`。

```java
Optional<String> nonEmptyOptional = Optional.of("Hello, World");
```

#### 2. 使用 `Optional.ofNullable()`

`Optional.ofNullable()` 用於創建一個可能包含 `null` 值的 `Optional` 對象。如果傳入的值為 `null`，則返回一個空的 `Optional` 對象。

```java
Optional<String> nullableOptional = Optional.ofNullable(null);
```

#### 3. 使用 `Optional.empty()`

`Optional.empty()` 用於創建一個空的 `Optional` 對象。

```java
Optional<String> emptyOptional = Optional.empty();
```

### 檢查 `Optional` 的值

#### 1. 使用 `isPresent()`

`isPresent()` 方法返回一個布爾值，表示 `Optional` 是否包含值。

```java
if (nonEmptyOptional.isPresent()) {     System.out.println("Value is present"); }
```


#### 2. 使用 `ifPresent()`

`ifPresent()` 方法接受一個 `Consumer`，如果 `Optional` 包含值，則執行給定的代碼塊。

```java
nonEmptyOptional.ifPresent(value -> System.out.println("Value: " + value));
```

### 獲取 `Optional` 的值

#### 1. 使用 `get()`

`get()` 方法返回 `Optional` 中包含的值。如果 `Optional` 為空，則拋出 `NoSuchElementException`。

```java
String value = nonEmptyOptional.get();
```

#### 2. 使用 `orElse()`

`orElse()` 方法在 `Optional` 為空時提供一個默認值。

```java
String value = nullableOptional.orElse("Default Value");
```

#### 3. 使用 `orElseGet()`

`orElseGet()` 方法在 `Optional` 為空時，調用一個 `Supplier` 並返回其結果。

```java
String value = nullableOptional.orElseGet(() -> "Default Value from Supplier");
```

#### 4. 使用 `orElseThrow()`

`orElseThrow()` 方法在 `Optional` 為空時，拋出一個指定的異常。
```java
String value = nullableOptional.orElseThrow(() -> new IllegalArgumentExcepti
```


## Summary

>**核心原則**：Optional 是為了**方法返回值**設計的，用來明確表達「這個方法可能返回空值」。把它當作避免 NPE 的工具，而不是 null 的完全替代品。

## Reference

[Java Optional 基本 & 心得分享 - 艾倫的開發心路歷程](https://allenhsieh1992.com/posts/java/java-optional-basic/)