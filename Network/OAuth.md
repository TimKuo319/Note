
## What is OAuth , why we need it

OAuth（Open Authorization) 是一種開放標準協議

目的在於讓第三方應用程式在`不需要知道使用者密碼的情況下`，取得用戶對於特定平台( `facebook` `google`)的有限訪問權。


## Role in OAuth

+ Resource Owner
	+ 擁有第三方平台應用程式使用權限的使用者
	+ Ex : `google` 或 `facebook` 的使用者

+ Client
	+ 客戶端是指`希望訪問資源擁有者在資源伺服器上的資源的應用程式`
	+ Ex : 我們的應用程式

+ Authorization Server
	+ 負責驗證資源擁有者的身份並授權客戶端的伺服器。這通常是 OAuth 提供者的一部分
	+ Ex: Google 的 OAuth 授權伺服器

+ Resource Server
	+ 資源伺服器是保存資源擁有者的資源並向經授權的客戶端提供資源的伺服器。這可能是與授權伺服器相同的伺服器或一個單獨的伺服器。

## Auth Flow

1. 應用程式請求授權
	+ 用戶在應用程式中點擊第三方登入
	
2. 用戶被導向到第三方平台(`google` or `facebook` etc) 的授權頁面，並在頁面中授予應用程式訪問權

3. 取得授權碼
	+ 當用戶同意授權後，第三方平台的 Authorization Server 會將用戶`redirect`回應用程式，並`在 URL 中附上授權碼(Authorizatoin code)`

4. 交換 `access token` 
	+ 當應用程式在第三步驟收到 `Authorization code` 之後，就需要再將`Authorization code`傳回給第三方平台，交換`access token`

5. 資料索取
	+ 當成功在第四步驟中拿到第三方平台的 `accesstoken`後，之後就可以透過這個`access token`來訪問使用者在第三方平台上的資源。
	

## 註冊 OAuth 常見欄位

+ 應用程式網域 - App Domains
	+ 填寫你的 application domain name
	+ 其實就是告訴第三方平台，`是誰要使用 OAuth 服務`
	+ 只有第三方平台驗證過的 domain name 才可以使用他們的 auth 服務

+ 授權網域 - Authorized Domains
	+ 一個 `domain name list`，list 中的網域被允許發起 OAuth 請求
	+ 當一個應用程式有很多 `sub domain` 時，可以允許這些 domain 進行 OAuth 操作

> [!info] App Domain vs Authorized Domain
> App Domain 是一個`明確的單一網域`，是應用程式在的位置
> 
> Authorized Domains 是一個列表，`指定了可以進行 OAuth 訪問的 domain`

+ Authorized JavaScript Origin
	- 允許發起OAuth請求的完整URL列表（包括 `protocol`和 `port`）
	- 主要用於客戶端JavaScript應用的OAuth流程
	- 提供更精細的控制，特別是針對OAuth請求
	- 可包括開發環境URL（如localhost）

+ Authorized Redirect URIs
	- 接收來自 OAuth provider 的 `authorization code` 的 endpoint，需要再發送回去請求 `access_token`

## Reference

+ https://ithelp.ithome.com.tw/m/articles/10270271

+ [[Note] OAuth2.0 筆記 | PJCHENder 未整理筆記](https://pjchender.dev/internet/note-oauth2/)