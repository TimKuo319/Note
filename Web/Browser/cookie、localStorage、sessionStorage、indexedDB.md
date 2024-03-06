---
date: 2024-03-06 Wed 21:01
---
---

1. *Cookie*
	+ 時效
		+ server透過 ==Expires或Max-Age設定時效==
	+ 用途
		+ session管理、購物車、遊戲資訊等等
		+ 個人化設定、主題
		+ 紀錄並分析使用者的行為
	+ 儲存大小
		+ 4KB
	+ 特點
		+ 每次存取都會連同request一起被發送，可能減少效率
		+ 每個domain name下的cookie數量有限制
	
2.  *LocalStorage*
	+ 時效 
		+ 永久儲存在瀏覽器中
	+ 用途
		+ 儲存一些網站相關的靜態資源，加速下次載入效率
		+ 離線web應用暫存
	+ 存取
		+ 只有來自同源的來源能夠存取對應的localstorage資料
	+ 儲存大小
		+ 5~10MB
	
3.  *SessionStorage*
	+ 時效
		+ 在該分頁或瀏覽器被關閉後就會自動被清除
	+ 用途
		+ 適用於儲存單次使用時要用到的資訊，ex: *表單數據*
	+ 存取
		+ 只有當前瀏覽器標籤存取的到
	+ 儲存大小
		+ 5~10MB

==以上三者皆只能儲存string格式的data==

4. IndexedDB
	+ 學習曲線相對較高
	+ 時效
		+ 需要手動清理或透過API方式清除
	+ 用途
		+ 在數據型態更加複雜，或儲存空間更大的時候可以使用
	+ 存取
		+ 只能透過同源存取
		+ 還有其他限制可見[IndexedDB - Web APIs | MDN (mozilla.org)](https://developer.mozilla.org/zh-TW/docs/Web/API/IndexedDB_API)
	+ 儲存大小
		+ 250MB以上，受電腦及瀏覽器供應商等可能有所不同


*註: 在做使用者驗證時server端所使用的session跟這裡的session storage是不一樣的，seesion通常會透過server生成sessionID儲存在==cookie==中，而這裡的seesionStorage是用來儲存一些單次使用時可能會使用到的資料*

# Reference

[HTTP cookies - HTTP | MDN (mozilla.org)](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Cookies)
[IndexedDB API - Web APIs | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
[你知道 localStorage 但你知道 IndexedDB 嗎？｜ExplainThis](https://www.explainthis.io/zh-hant/swe/what-is-indexeddb)
