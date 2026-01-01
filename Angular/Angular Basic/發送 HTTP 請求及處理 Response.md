
## Intro

要作為一個可用的前端應用程式，必然會少不了與後端溝通的過程，而這部分就是透過 API 來進行溝通。


## HttpClient 

HttpClient 是 angular 提供用來進行 http 請求的介面，有許多種配置，像是`withInterceptors`、`withJsonSupport`... 等，可以參考官方文件 [Setting up HttpClient • Angular](https://angular.dev/guide/http/setup)。

HttpClient 中通常會在 `app.config.js` 中做設定，程式碼如下，設定完成後即可在要使用的地方(通常會是`service`，因為會將呼叫 API 的邏輯統一集中在 service 中)。

```ts
export const appConfig: ApplicationConfig = {
  providers: [provideHttpClient()],
};
```


>[!info]
> HttpClient 與我們自訂義撰寫的服務都可以透過 inject 的方式被注入使用。但為何 HttpClient 需要到 `app.config.ts` 中進行設定呢？
> 在我們自訂義的 service 中，通常會在上方透過 `providedIn: ...` 






- http 和 router 先在 AppConfig 註冊註冊與否有什麼差別？有沒有註冊好像都可以在 component 直接被 import 做使用？ 
	- 對於一般我們常使用的 service，本身因為已經設定好 provider，可以直接注入，但對於 http client 這些邏輯最先在 app 中沒有被設定到 provider，所以需先到 AppConfig 中設定
		- [ ] 補充單一元件做法


- http client 預設回傳 response body 的內容
- http client 的 method 還具備第二個參數，`observe` 能夠用來觀察不同的情況
	- `observe`
		- `response`
			- 從 next 中會得到的 response 物件，包含 header、status code、body 等
		- `event`
			- 在 request - response lifecycle 中接收到任何 event 時就會觸發．

- 可使用 `catchError()` operator 來進行轉換邏輯，提升在  `error()` 中的可讀性

- `$event`
	- https://angular.dev/guide/templates/event-listeners#accessing-the-event-argument

- optimistic update 
	- 在前端收到使用者資料時就先針對畫面進行更新，來讓使用者能夠立即得到回饋
	- 這樣的做法預設是與 backend 的請求會成功，但在錯誤時會導致前端與後端不一致，所以要針對請求的錯誤狀況進行 rollback 處理


- [ ] http client 的 query parameter、path parameter 使用方式

- [ ] service 層除了處理 httpClient 之外，還會做哪些事

- [ ] interceptor 影片待補 - udemy angular 課程 12 節 238 