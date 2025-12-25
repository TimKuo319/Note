

- Subject 
	- 一種特殊類型的 observable
	- 需手動發出值
	- multicast  

- Observable
	- 不一定有初始值
	- 隨時間變化的值
		- observable 會隨時間不斷發出新的 data
	- 適合用來接收資料流 / 處理非同步事件
		- 為何適合處理非同步事件？
			- 因為 observable 只有在被 subscribe 的時候才會啟動，對於不會馬上收到會傳結果的非同步事件來說，當事件處理完畢再 subscribe 就是適合 observable 特性的場景。
	- unicast 
	- push-based

- Signal
	- 有初始值
	- 值容器
	- 適合處理同步事件
	- 適合用來作為狀態管理
	- effect
		- 當其中內部讀的 signal 改變時，重新自動執行
	- computed
	- pull-based


- 利用 `destoryRef` 來解除訂閱

## Observable vs Signal 差異對比表

| 特性              | Observable (RxJS)                                                | Signal (Angular)                               |
| --------------- | ---------------------------------------------------------------- | ---------------------------------------------- |
| **資料流模型**       | 推送式（Push-based）<br>Observable 主動推送值給訂閱者                          | 拉取式（Pull-based）<br>需要時才讀取當前值                   |
| **訂閱機制**        | 需要手動 `subscribe()`                                               | 自動追蹤依賴關係，無需訂閱                                  |
| **記憶體管理**       | 需要手動 `unsubscribe()` 避免洩漏                                        | 自動清理，無需手動管理                                    |
| **變更偵測**        | 觸發整個元件樹的變更偵測                                                     | 細粒度更新（只更新使用該 Signal 的地方）                       |
| **時間序列處理**      | ✅ 擅長處理非同步、多個值的串流                                                 | ❌ 只表示當前狀態值                                     |
| **學習曲線**        | 較陡峭（需理解 RxJS operators）                                          | 較平緩（簡單直觀的 API）                                 |
| **效能表現**        | 可能有訂閱洩漏風險                                                        | 更高效的變更偵測機制                                     |
| **錯誤處理**        | ✅ 內建錯誤處理（`catchError`）                                           | ❌ 無內建錯誤處理                                      |
| **取消操作**        | ✅ 支援取消訂閱（`unsubscribe`）                                          | ❌ 不支援取消                                        |
| **多值發送**        | ✅ 可隨時間發送多個值                                                      | ❌ 只能表示單一當前值                                    |
| **Template 使用** | 需使用 `async` pipe<br>`{{ data$ \| async }}`                       | 直接呼叫函式<br>`{{ data() }}`                       |
| **衍生計算**        | 使用 operators<br>`map()`, `filter()` 等                            | 使用 `computed()`<br>自動重新計算                      |
| **初始值**         | 不一定需要初始值                                                         | 必須提供初始值                                        |
| **適用場景**        | • HTTP 請求<br>• WebSocket<br>• 事件流<br>• 時間相關操作<br>• 複雜資料流轉換       | • 元件狀態管理<br>• 衍生計算<br>• 同步狀態值<br>• Template 綁定 |
| **程式碼範例**       | `data$ = this.http.get('/api')`<br>`data$.subscribe(v => {...})` | `data = signal(0)`<br>`value = data()`         |
| **何時使用**        | 非同步操作、時間序列、複雜流程                                                  | 當前狀態、衍生計算、效能優化                                 |
### 快速決策指南

| 需求                   | 選擇                      |
| -------------------- | ----------------------- |
| 表示當前狀態值              | **Signal**              |
| HTTP 請求              | **Observable**          |
| 需要衍生計算               | **Signal** (`computed`) |
| 需要 debounce/throttle | **Observable**          |
| Template 綁定          | **Signal** (效能更好)       |
| WebSocket 即時資料       | **Observable**          |
| 簡單狀態管理               | **Signal**              |
| 複雜非同步流程              | **Observable**          |
| 需要取消操作               | **Observable**          |
| 自動記憶體管理              | **Signal**              |


## Reference

- [Observable and Subjects in Angular | by Jaydeep Patil | Medium](https://medium.com/@jaydeepvpatil225/observables-and-subjects-in-angular-a4d73dfa5bb)

- [Course: Angular - The Complete Guide (2025 Edition) | Udemy](https://www.udemy.com/course/the-complete-guide-to-angular-2/learn/lecture/44126630#overview)