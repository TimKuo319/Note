
---

在Js中，一個function的前方搭配上`async`關鍵字的時候，表示這個function最終會回傳一個`resolved的promise`。利用`async`搭配上`await`時，可以減少過去用許多`then`、`catch`所造成的可讀性低的問題。

原始方式
```javascript
function getUser() { 
	fetch('/users') .then(response => response.json())        
	.then(user => { 
	// 使用 user 
	}) 
	.catch(error => { 
	// 處理錯誤 }); 
	}
```


搭配`async`及`await`後的方式，將fetch這個promise搭配await來等待`resolved`的結果，若是promise結果為`reject`，則會因為丟出error而交給catch處理

```javascript
function getUser() {
	try{
		let response = await fetch('/users')
		let user = response.json
	}
	catch(err){
		//錯誤處理
	}
}
```