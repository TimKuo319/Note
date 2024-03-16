---
date: 2024-03-16 23:28 Fri
---
---

# Redis

Redis比較常被使用的情境是用來作為cache server，但他本身同時也是一種NoSQL資料庫。
## Category

根據官方文件，redis可以依照用途分為三大類儲存結構
+ Data structure store
	+ 最常見的使用場景，透過string、hash 、list、set等等來做cache、counter、queue等功能
+ Document structure store
	+ 作為文件導向的資料庫，可以將json格式的文件儲存在其中
+ Vector database
	+ 用來儲存向量數據，可以做為儲存推薦系統、影像搜索等等有指向性資料的資料庫

## Datatype

redis提供了許多類型的資料結構可以供我們儲存資料，以下列出一些基本常使用的類型與指令。

+ String
	+ Set key value
		+ opt
			+ nx
				+ 只有當key不存在的時候才會創立
			+ xx
				+ 只有當key存在的後才會創立
			+ ex
				+ 用來設定該筆資料過期的時間，以秒為單位
				+ 對於一些==時效性的資料==像是session，可以利用ex來幫我們自動刪除
	+ Get key value
+ Lists
	+ LPUSH key value 
		+ 在head增加新資料
	+ LPOP key value
		+ 從head移除資料，並回傳該筆資料
	+ RPUSH key value
		+ 相反於LPUSH
	+ RPOP key value
		+ 相反於LPOP
	+ LRANGE key start stop
		+ 可以用來查詢list的狀況，start為起始index，stop則為終點index
		+ -1可以作為start可以表示結束，所以LRANGE key 0 -1可以查詢整個list
+ Sets
+ Hash
	+ 適合用於多屬性物件資料
+ Sorted Set
	+ 適合用於需要排序的場景，ex: 排行榜，數據統計

## Reference
[Redis](https://redis.io/)
