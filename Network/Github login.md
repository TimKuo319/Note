

- spring security 會處理的特定 endpoint
	- 導向授權
		- `/oauth2/authorization/github`
	- redirect-uri
		- `/login/oauth2/code/github`

- security 的 session 通常會用來保存用戶資訊

- 踩雷點 

	- session 不能設定為無狀態，因為需要用來儲存使用者資訊



