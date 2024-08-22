
### What is sharding , why sharding

因為在只有一個 DB 的狀況下，即時機器的效能再好，最終也會遇到瓶頸，因為一個機器的 throughput 是有上限的。

解決辦法就是分為多個 DB ，`透過增加 DB 的數量，來解決吞吐量的問題。`

做法是會將原先的資料庫拆成多等分，每一個 shard 都拿到`其中一等分`

又或者是原先的資料量太大，垂直擴展的成本比水平擴展的成本還要昂貴，所以透過 sharding 的方式來解決這個問題。


### Pros and Cons

+ Pros
	+ 透過將資料分散到不同的 DB ，可以降低單一資料庫中的 DB 數量，提高查詢速度
	+ 一個 shard 出現故障的話，其他 shard 還可以正常運作
	+ 透過水平擴展取代垂直擴展 -> 降低成本

 + Cons
	 + 增加資料存取的複雜度
	 + 擴充性低 (可透過 `consistent hash` 解決這個問題)
	 + lock 耗時，unique key 較省時間

### Reference

[【筆記】Database Sharding. （由於用途為筆記，不會像以往文章太詳細介紹概念） | by 莫力全 Kyle Mo | Medium](https://oldmo860617.medium.com/%E7%AD%86%E8%A8%98-database-sharding-22e22f0809c0)

https://homuchen.com/posts/what-is-database-partition-sharding/