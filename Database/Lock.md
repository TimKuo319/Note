

- [ ] lock note

- [ ]  read lock vs write lock
	- 會 block 住哪些操作
	- 在不同 isolation level 會有什麼影響
	- read lock
		- read commited 
			- 直接讀取最新值，造成 non-repeatable read
		- repeatable read
			- 透過 snapshot 達到 transaction 內一致性
			

- [ ] 樂觀鎖 vs 悲觀鎖

[【MySQL数据库原理 零】MySQL数据库原理看这一篇就够了_mysql的xid是保序的吗-CSDN博客](https://blog.csdn.net/sinat_33087001/article/details/114715323)

[淺入淺出 MySQL Ep5 : 是誰卡住你的 Transaction？Lock 是不是你？ | by vic | Medium](https://vicxu.medium.com/%E6%B7%BA%E5%85%A5%E6%B7%BA%E5%87%BA-mysql-ep5-%E6%98%AF%E8%AA%B0%E5%8D%A1%E4%BD%8F%E4%BD%A0%E7%9A%84-transaction-lock-%E6%98%AF%E4%B8%8D%E6%98%AF%E4%BD%A0-f7f74db3a7a3)

[iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/m/articles/10323593)