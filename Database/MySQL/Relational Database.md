
## Normalization

+ 將entity的描述資料，透過一定的程序將table簡化，直到一張表只描述一個單純的事實為止


## Keys

+ Unique keys
	+ 唯一值
	+ 可以是Null
	+ 每個table可以有很多unique key
	+ 可以被`修改為一個新的值`
	
+ Primary keys
	+ `不能是null`
	+ 一張table只能有一個
	+ `不能`被修改

+ Foreign keys
	+ Foreign Key的值`必須是另外一張表中的存在的primary key`
	+ 用來跟其他table做關聯


## RelationShip

+ one-to-one
	+ 資料表之間的row是一對一對應的
	+ 使用者帳密對應到使用者詳細資訊
	+ 用來提升效能
		+ 將frequently access data與infrequently access data分開

+ one-to-many
	+ 最常見的類型
	+ 一個客戶可以有多個訂單，但一筆訂單只能有一個客戶
	+ primary key在many那邊
	
+ many-to-many
	+ 一個學生可以選修很多門課，一門課也可以被很多個學生選修
	+ 會依照兩張table的primary key再額外建一個資料表(`junction table`)儲存這個資訊
		+ 實際上就是變成兩個one-to-many的情況
## Reference

[Day 32 資料庫正規化(一~三) - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10229472)