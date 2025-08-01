---
tags:
  - LInux
---

## Basic Concept

Linux 指令一班會有三個輸入輸出流，分別為

1. 標準輸入 (`stdin` 代碼 `0`)  -> 預設從鍵盤讀取輸入
2. 標準輸出 (`stdout` 代碼 `1`)  -> 預設輸出到當前 shell
3. 標準錯誤輸出 (`stderr` 代碼 `2`) -> 預設輸出到當前 shell

### 標準輸出

我們常會使用像是

```sh
ls > file.txt
```

來將某指令的結果導向另一個檔案，在這種沒有指定的狀況下，預設就是將 `stdout` 導向檔案

等價於

```sh
ls 1> file.txt
```

### 標準錯誤輸出

上面指令是將標準輸出導向到某個檔案中，但並不會包含錯誤訊息，因為錯誤訊息的輸出是透過 `stdout` ，所以如果想要將 `stdout` 輸出到檔案中的話，就要明確指定輸出流。

```sh
ls 2> file.txt
```


而有時我們會想要將 `stdout` 與 `stderr` 輸出到同一個檔案中，這時就有點像在連接水管的感覺

```sh
ls > file.txt 2&>1
```

透過先將前面指令的 `stdout` 指向某個檔案，再將 `stderr` 導向 `stdout`，而因為前面的 `stdout` 已經導向 `file.txt`，所以就會將兩者都導向同一個檔案

## Reference

- [Linux I/O 輸入與輸出重新導向，基礎概念教學 – G. T. Wang](https://blog.gtwang.org/linux/linux-io-input-output-redirection-operators/)