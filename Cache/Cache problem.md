---
date: 2024-03-25 15:00 Mon
---
---
## cache penetration (快取穿透)
==cache本身並無作用==，系統依舊要透過database存取資料。

1. database本身就沒有資料: 正常狀況下，這樣的請求量通常不會太大，但如果被惡意攻擊則有可能拖慢系統。
	+ ==解決辦法==
	+ 當搜尋的key重複率較高時，把相對應的key值給一個default，下次再被存取到時候就會回傳空值。
	+ 當搜尋的key重複率較低時，利用`bloom filter`，若存在則去cache或database拿取，反之則回傳null。
	+ 
2. cache資料須要花大量的時間: 沒辦法把所有商品存到cache中，會需要分頁存，所以可能在訪問cache的時候會失效，進而使cache無法起到作用。
## cache avalanche (快取雪崩)

若是所有的key都設定同一個exipred time，則當時間一到，所有的key同時過期時，這所有的請求都會需要導向資料庫，就會造成資料庫壓力過大。
+ 可以利用一定區間的隨機值來當作expried time。

## cache breakdown 

==cache失效後引起系統效能急遽下降的情況。若此時有大量訪問的請求都去向資料庫索取資料就會造成問題。==

+ 更新鎖 
	+ 保證只有一個線程可以更新cache，其他線程則等待
+ 後臺更新

## hotspot cache

儘管cache本身性能較高，但對於一些比較常被存取的資料(hotspot)，如果大部分的請求都命中同一份資料，則對該資料所在的cache server壓力也會很大。

可以透過透過複製該資料到多台cache server，減輕單台server的壓力，*但cache副本的expired time也要不一樣，避免cache avalanche*

*dirty data : 指那些在database中已經被更新的資料，但在cache中並沒有更新，導致應用程式讀取的資料時讀取到不精準的資料。*
## Reference

[什麼是快取 (Cache)？快取 (Cache) 的機制為何？｜ExplainThis](https://www.explainthis.io/zh-hant/swe/cache-mechanism)
