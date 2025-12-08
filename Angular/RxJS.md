

- 有多種 subject 可以作為狀態控制
	- 可以將 subject 想像成信號
- 如果訂閱了 subject，通常會註冊 call back function
	- 需要在 `onDestory` 中解除訂閱，避免 callback function 留存在記憶體中造成 memory leak
	- 因為 angular 在訂閱關係存在時是不會做 gc 的