

---

## 四大元件

+ Call Stack
+ Web APIs
+ Task Queue
+ Microtask Queue
### Call Stack(FILO)

+ 對於non-block的程式就直接執行
	+ `console.log`
	+ 一般的function
+ 剩餘的則依照狀況分配給不同的其他元件
### Web APIs

+ 對於一些需要等待的瀏覽器操作，會將這些指令交給瀏覽器去做執行，我們的程式碼則繼續執行Call Stack內的東西
	+ fetch
	+ settimeout

### Task Queue(FIFO)

+ 當Web APIs執行完得到結果後，就會將這些結果放到Task Queue中，等到Call Stack執行完畢後，`再將Task Queue的內容放到Call Stack內執行`

### Microtask Queue(FIFO)

+ 對於callback function，在某些call back function執行完畢後，將他們放到task queue中，`一樣等到Call stack清空後，再執行Microtask Queue`
	+ 一般的callback function
	+ then區塊內的function
	+ catch區塊內的function

+ 當Task Queue與Microtask Queue都有內容需要執行時，會優先清空Microtask Queue再執行Task Queue

## Reference

https://www.youtube.com/watch?v=eiC58R16hb8