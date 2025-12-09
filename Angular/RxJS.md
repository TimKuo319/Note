

- 有多種 subject 可以作為狀態控制
	- 可以將 subject 想像成信號
	
- 如果訂閱了 subject，通常會註冊 call back function 到記憶體中，而因為 call back function 會持有 component 的 ref，會造成 gc 無法自動銷毀 component
	- 需要在 `onDestory` 中解除訂閱，避免 callback function 留存在記憶體中造成 memory leak
		- 可使用 `destory$` 搭配 `takeUntil(this.destory)`
		- 使用 `takeUntilDestoryed`
	- 因為 angular 在訂閱關係存在時是不會做 gc 的

- `complete` vs `unsubscribe`
	- `complete` 是由 observable 主動發出，當 complete 時，就會自行取消所有訂閱
	- `unsubscribe` 是由訂閱者發出，發出時解除訂閱關係，但 observable 還可以繼續發送值 


### API Call 基本結構

```ts
this.http.method(url, data).pipe(
  // Operators（可選）
  finalize(() => { /* 一定會執行 */ }),
  takeUntilDestroyed()
).subscribe({
  next: (response) => {
    // 處理成功回應
  },
  error: (error: HttpErrorResponse) => {
    // 處理錯誤
  },
  complete: () => {
    // HTTP 成功時會執行（可選）
  }
});
```