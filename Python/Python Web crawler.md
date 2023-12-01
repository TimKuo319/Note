---
date: 2023-05-02 23:22
---
可利用prority queue來對不同url設定優先度
設定延遲避免造成伺服器過大負荷

## Avoid being blocked

+ 一直改變`User-agent` -> 不讓伺服器判定為機器人
+ 在伺服器離峰時段配合延遲使用，來降低伺服器負荷
+ 遵照`robots.txt`，不去爬取特定網頁
+ 避免`honeytraps`，有些連結是特別用來辨認是否為機器人的，避免爬取那些連結。

## Efficiency

+ Parallel Scraping in Python with Concurrency("[Web Crawler in Python: Step-by-Step Tutorial 2023 - ZenRows](https://www.zenrows.com/blog/web-crawler-python#best-web-crawling-practices)")
+ Distributed Web Scraping in Python
+ Separation of Contents for Easier Debugging
+ 在遇到帶有`query string`的url時，應先將`query string`或`hash`欄位刪除，避免爬到重複的網頁。