

- Subject 
	- 一種特殊類型的 observable
	- 需手動發出值
	- multicast  

- Observable
	- 不一定有初始值
	- 隨時間變化的值
		- observable 會隨時間不斷發出新的 data
	- 適合用來接收資料流
	- unicast 
	- push-based

- Signal
	- 有初始值
	- 值容器
	- 適合用來作為狀態管理
	
	- effect
		- 當其中內部讀的 signal 改變時，重新自動執行
	- computed
	- pull-based


- `observable` vs `signal` 使用場景
	- `observable` 用來處理事件接收與資料
	- `signal` 用來進行狀態管理

- 利用 `destoryRef` 來解除訂閱
## Reference

- [Observable and Subjects in Angular | by Jaydeep Patil | Medium](https://medium.com/@jaydeepvpatil225/observables-and-subjects-in-angular-a4d73dfa5bb)