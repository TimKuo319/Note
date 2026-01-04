

- [ ] angular getter 是什麼


- 可以透過 `zone.runOutsideAngular` 來針對不會影響 UI 的事件減少 change on detection cycle

- `default` vs `onPush`  detection strategy 
	- `default` 預設情況下會在有 component 接收到`event`、 `input 變更`、`HTTP Request`、`Promise`等狀況時重新檢查所有 component，更精確地來說，是會在**非同步操作**完成時，觸發 `zone.js` 做 chagne on detection。

	- 原理
		- **所有非同步操作最終都會回到主線程**
		- Zone.js 在這些「回到主線程」的時機點做標記
		- Angular 在這些時機點統一檢查變化並更新
		- [zone.js - How it works in Angular?](https://angular.love/from-zone-js-to-zoneless-angular-and-back-how-it-all-works)
		- [A change detection, zone.js, zoneless, local change detection, and signals story 📚 - justangular.com](https://justangular.com/blog/a-change-detection-zone-js-zoneless-local-change-detection-and-signals-story)

	- 當對一個 component 使用 `onPush` 時，只有當這個 component 與他的 child component 有上述變動時才會被重新檢查
		- 也就是說，當其他不是自己 component 的變動時，是不會被重新檢查的，以此來達到提升效能的效果
	
- `onPush` 問題
	- 在使用 `onPush` 時，對於 component 來說，如果遇到像是接收 API 回傳內容等方式時，因為不屬於 input change、也不是事件，在沒有手動呼叫 `markForCheck` 的狀況下．如果沒有透過 `signal` 或 `observable` 來處理的話，就會造成沒有被 change detection 檢查到而沒有觸發重新渲染． 