


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




- [ ] http client 的 query parameter、path parameter 使用方式


- [ ] service 層除了處理 httpClient 之外，還會做哪些事