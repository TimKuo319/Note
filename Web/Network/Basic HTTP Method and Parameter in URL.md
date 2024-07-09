
### Parameter in URL

+ Path Variables
	+ 通常用於存取resources，存取唯一的資料
		+ 比較常在`GET method`時使用，如果是`POST method`會將變數放在`reqeust body`，所以不會使用path variable
	+ 格式
```bash
/users/123/orders/456
```

+ Query Parametes
	+ 通常用於過濾和數據排序，可以根據不同的參數組合進行變動，且==不會影響到路徑變化==

### 常見HTTP Method

+ GET
	+ 會將要傳遞的資料以`parameter`的形式放在URL中，傳遞給server
	+ 通常不具備`reqeust body`
	+ 主要用來取得資料
	+ `cacheable`
+ POST
	+ 會將要傳遞的資料放在`request body`中
	+ 通常用來傳送資料
	+ 因為每次傳遞的資料可能都會不一樣，所以`non-cacaheable`
+ PUT
	+ 用來執行修改操作(即時有相同的資料也會全部覆蓋)
	+ `non-cacheable`
+ DELETE
	+ 用來執行刪除操作
	+ `non-cacheable`
+ PATCH
	+ 通常用來執行修改操作(只針對要修改的部分進行修改)
	+ `non-cacheable`

*Reference: [淺談 HTTP Method：表單中的 GET 與 POST 有什麼差別？](https://blog.toright.com/posts/1203/%E6%B7%BA%E8%AB%87-http-method%EF%BC%9A%E8%A1%A8%E5%96%AE%E4%B8%AD%E7%9A%84-get-%E8%88%87-post-%E6%9C%89%E4%BB%80%E9%BA%BC%E5%B7%AE%E5%88%A5%EF%BC%9F)*

### URL vs URI

+ URL (Universal Resource Locator)
	+ 標識一個網路資源，指定對其進行操作或訪問的方法
	+ `https://developer.mozilla.org/en-US/docs/Learn/`
+ URI (Universal Resource Identifier)
	+ 對資源進行標識
	+ 最簡短可能連訪問方法都沒有
	+ `/users/123`

+ 所以URL其實是包含在URI的範圍內，因為URL中也一併包含了資源的位置
	+ 所有URL都是URI，其逆不真。






