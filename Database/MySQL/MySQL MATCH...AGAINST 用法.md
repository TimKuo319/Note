
## FULLTEXT index

在提到 match against 之前，要先提到 FULLTEXT index。FULLTEXT index 是 MySQL 中一種 index 的種類，針對 `CHAR/VARCHAR/TEXT` 型態建立 index，用於加速全文搜尋的操作。

通常採用 **倒排索引** 的索引結構。

## Match ... AGAINST

使用方式：

```sql
SELECT ... FROM table
WHERE MATCH(col1, col2, ...)
      AGAINST('搜尋字串' [IN NATURAL LANGUAGE MODE | IN BOOLEAN MODE | WITH QUERY EXPANSION]);

```

`MATCH 的後方填入要搜尋的欄位，AGAINST 後方填入要搜尋的字串及對應的搜尋模式`

- Natural Language Mode
	- MySQL 會將查詢字串拆成 token，只有**完全配對**到 token 的欄位才會被回傳。回傳結果依照關鍵字出現的頻率及相關性做排序，token 不包含 `stopwords`
	- 包含更多 token 或出現頻率較高的資料，其 relevance 通常更高。
	- EX:搜尋 `"apple banana cherry"`，只要資料中包含 `apple`，或 `banana`，或 `cherry` 中任一 token，就會被回傳。
	
- Boolean Mode
	- 可以透過一些符號去做搜尋目標調整，也能夠做到模糊搜尋，但是只能做 `prefix` 的模糊搜尋。 
	- `+` : 必須存在某個字
	- `-` : 必須不存在某字
	
- Query Expansion
	- 查詢擴展模式，先用自然語言搜尋取得初步結果，再以這些結果中的關鍵詞擴張搜尋一次

## MATCH AGAINST + ngram parser

如果想要透過 MATCH AGAINST 做到模糊搜尋的的話，必須在建立 FULLTEXT index 時再加上 `with parser ngram` 來使用。

加上 `with parser ngram` 會讓 MySQL 針對目標欄位去拆 token，預設的 token size 為 2，以某欄位的值為 mysql 來說，可能就會被拆成 `my`, `ys`,`sq`,`ql` 等 token。

當今天有一個搜尋的請求是 `mys` 時，也會將這個搜尋字串拆成 `my`, `ys` 去比對透過 FULLTEXT index 建立時拆分出來的 token，來達成模糊搜尋的目的 (不論是哪一個 mode)。

## 對比 LIKE 的模糊搜尋差異

一般來說，如果我們希望做到模糊搜尋的話，通常會希望是前中後綴都可以符合，但是 LIKE 在進行中後綴搜尋的時候會因為沒辦法知道前綴是什麼，而無法使用到 index，所以會導致 full table scan 造成低效能。

而且 LIKE 並沒有提供匹配的關聯度，所以在需要依照搜尋結果的關聯度排序的話，使用 match against 相對會是比較合適的做法。

當然，使用 ngram parser 的話，會因為需要針對欄位拆分成多個 token，而使得建立 index 需要較大的開銷，但在資料量大情況下，相對 full table scan 依舊會是效能比較好的方案。

### 搜尋行為差異

#### LIKE 精確匹配
```sql
-- 精確匹配連續字符串
WHERE title LIKE '%MySQL資料庫%'
-- 只匹配包含完整 "MySQL資料庫" 的記錄
```

#### N-gram Token 匹配
```sql
-- N-gram 分解匹配
WHERE MATCH(title) AGAINST('MySQL資料庫')
-- 內部處理：["My", "yS", "SQ", "QL", "L資", "資料", "料庫"]
-- 可能匹配包含部分 tokens 的記錄
```

## Reference

- [Full Text Search in MySQL - DEV Community](https://dev.to/bbkr/full-text-search-in-mysql-5g1i)


