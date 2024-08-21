
### What is sharding , why sharding

因為在只有一個 DB 的狀況下，即時機器的效能再好，最終也會遇到瓶頸，因為一個機器的 throughput 是有上限的。

解決辦法就是分為多個 DB ，`透過增加 DB 的數量，來解決吞吐量的問題。`

做法是會將原先的資料庫拆成多等分，每一個 shard 都拿到`其中一等分`





### Reference

[【筆記】Database Sharding. （由於用途為筆記，不會像以往文章太詳細介紹概念） | by 莫力全 Kyle Mo | Medium](https://oldmo860617.medium.com/%E7%AD%86%E8%A8%98-database-sharding-22e22f0809c0)