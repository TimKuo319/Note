

- Subject 
	- 一種特殊類型的 observable
	- 需手動發出值

- Observable
	- 不一定有初始值
	- 隨時間變化的值
	- 適合用來接收資料流

- Signal
	- 有初始值
	- 值容器
	- 適合用來作為狀態管理
	
	- effect
		- 當其中內部讀的 signal 改變時，重新自動執行
	- computed