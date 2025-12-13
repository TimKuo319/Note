

- [ ] angular getter 是什麼


- 可以透過 `zone.runOutsideAngular` 來針對不會影響 UI 的事件減少 change on detection cycle

- `default` vs `onPush`  detection strategy 
	- `default` 預設情況下會在有 component 接收到事件、 input 變更等狀況時重新檢查所有 component
	- 當對一個 component 使用 `onPush` 時，只有當這個 component 與他的 child component 有上述變動時才會被重新檢查
		- 也就是說，當其他不是自己 component 的變動時，是不會被重新檢查的，以此來達到提升效能的效果
	
- `onPush` 問題
	- 在使用 `onPush` 時，對於 component 來說，如果遇到像是接收 API 回傳內容等方式時，因為不屬於 input change、也不是事件，在沒有手動呼叫 `markForCheck` 的狀況下．如果沒有透過 `signal` 或 `observable` 來處理的話，就會造成沒有被 change detection 檢查到而沒有觸發重新渲染． 