

## Breakpoint 種類

- `Line Breakpoint`
	- 最常見的 breakpoint 類型，在行內的中斷點，可以檢查當前行程式碼呼叫的 function 執行或是變數狀況

- `Method Breakpoint` 
	- 能夠在 method 進出時自動設立中斷點進行檢查，就可以不用在 return 前自己手動加上斷點，在 intellij 中會以紅色菱形顯示，右鍵後就可以選擇要不要在 function 退出前加上斷點。
	- **通常會放在 interface 的 method 上在多實作時去檢查當前使用到哪個實作**


## 基本功能

- `Resume program`
	- 直接進入到下一個設定的中斷點

- `Step over `
	- 不會深入當前行程式碼內的函式，會視為執行完後進到下一行

- `Step into`
	- 進到當前行的程式碼內的函式繼續執行，可以在 intellij 中設定 step into 的過濾名單避免進入到框架程式碼

- `Step out`
	- 退出方法，從進入的方法內退出到方法調用處，此時方法已執行完畢，只是還沒有完成賦值 
	- 舉例來說，在某行呼叫了 A method，透過 `step into` 進到 A method 中進行執行，假設沒有想要看完所有 A method 的所有內容，而想要他直接執行完，可以透過 `step out` 直接退出，只是此時 A method **尚未完成賦值**

- `Run to cursor`
	- 直接執行到滑鼠指定的行數


## Summary


1. 多在要驗證的地方加上中斷點，透過 `resume program`，減少頻繁 step over
2. 小範圍跳轉可以透過 `run to cursor` 

## Reference

[IntelliJ - Debug 的使用方法](https://kucw.io/blog/2020/2/intellij-how-to-debug/)