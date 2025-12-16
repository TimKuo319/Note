


- http 和 router 先在 AppConfig 註冊註冊與否有什麼差別？有沒有註冊好像都可以在 component 直接被 import 做使用？ 
	- 對於一般我們常使用的 service，本身因為已經設定好 provider，可以直接注入，但對於 http client 這些邏輯最先在 app 中沒有被設定到 provider，所以需先到 AppConfig 中設定
		- [ ] 補充單一元件做法