

- 什麼是 observable ?
	- stream of data 


	


- `complete` vs `unsubscribe`
	- `complete` 是由 observable 主動發出，當 complete 時，就會自行取消所有訂閱
	- `unsubscribe` 是由訂閱者發出，發出時解除訂閱關係，但 observable 還可以繼續發送值 



## 初始化方式

### of

```js
import { of } from 'rxjs';

of(10, 20, 30)
  .subscribe({
    next: value => console.log('next:', value),
    error: err => console.log('error:', err),
    complete: () => console.log('the end'),
  });

// Outputs
// next: 10
// next: 20
// next: 30
// the end
```


### from

```js
import { from } from 'rxjs';

const array = [10, 20, 30];
const result = from(array);

result.subscribe(x => console.log(x));

// Logs:
// 10
// 20
// 30
```

### fromEvent

```js
import { fromEvent } from 'rxjs';

const clicks = fromEvent(document, 'click');
clicks.subscribe(x => console.log(x));

// Results in:
// MouseEvent object logged to console every time a click
// occurs on the document.
```


### 自行透過 observable 建構子客製化 observable

對於訂閱者來說，在透過 `.subscribe()` 去訂閱 observable 物件之後，通常會透過 `next()`、`error()`、`complete()` 去接收 observable 產生的值。

而這些產生的值其實就來自於 observable 建構時的設定。

以下方的程式碼來說，customerInterval 實際上會每 1000ms 就產生一個 next 訊號給 subscriber，他的 subscriber 在透過 `next()` 接收時，所得到的就會是下方的 `message` 物件

```ts
customInterval$ = new Observable((subscriber) => {
setInterval(() => {
  subscriber.next({ message: 'New Value'});
}, 1000);
});

this.customInterval$.subscribe({
	// 這裏的 value 就會拿到 {message: 'New Value'}
	next: (value) => { ... }
})
```

## 取消訂閱

如果訂閱了 subject，通常會註冊 call back function 到記憶體中，而因為 call back function 會持有 component 的 ref，會造成 gc 無法自動銷毀 component。

為了解決這個問題，需要在 `onDestory` 中解除訂閱，避免 callback function 留存在記憶體中造成 memory leak，可以透過以下方式 `destoryRef`

同樣取用上方 customInterval 的程式碼


```ts
private destoryRef = inject(DestoryRef);

const intervalSubscription =  this.customInterval$.subscribe({
	// 這裏的 value 就會拿到 {message: 'New Value'}
	next: (value) => { ... }
})

this.destoryRef.onDestory(() => {
	intervalSubscriptoin.unsubscribe();
})

```



1. 可使用 `destory$` 搭配 `takeUntil(this.destory)`
2. 使用 `takeUntilDestoryed`




## API Call 基本結構

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


## 進階用法

- 客製化 observable
	- 包裝第三方套件
	- 



## Reference

- [from | Learn RxJS](https://www.learnrxjs.io/learn-rxjs/operators/creation/from)
- [RxJS - API List](https://rxjs.dev/api)