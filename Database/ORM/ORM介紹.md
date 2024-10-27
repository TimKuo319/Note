---
date: 2024-01-13 Sat 17:05
---
---
## Object Relational Mapping

最一開始看到ORM這個名詞是剛學mongodb的時候，當時因為是利用express.js進行開發，所以對應到就看到`mongoose`這個ODM，當時先簡單查詢一下後也一併查到了ORM這個名詞。

ORM就像是一個轉換器，透過程式語言以物件導向的方式去進行資料庫操作，就像[[MongoDB Conection and CRUD|MongoCRUD]]裡面的程式碼一樣。透過class建立data model，以此作為schema，利用像是`find()`等等套件提供的方式去拿取資料。

## Pros

### 1. 安全性

```php
$date = // 取得使用者傳來的日期資料  
$sql = "SELECT * FROM notifications WHERE date = " + $date;  
... 執行 SQL 語句
```

在以上的程式碼中，`date`是直接從使用者來的變數，若是駭客透過這個點進行SQL injection是有可能成功修改到資料庫內容的。而向下方的程式碼，是透過ORM來執行資料庫操作，在將`date`變數傳入ORM的function內時，ORM會去幫我們過濾一些奇怪的語法，來避免這點。

```php
$date = $_POST['date'] // 取得使用者傳來的日期資料  
Notification::where('date', $date)  
... 轉換成合法 SQL 語句，再執行
```
### 2. 簡化性

自己認為這個部分比較像是能夠讓程式碼看起來更統一，都是利用程式語言本身去進行操作，整體閱讀型會較佳。

### 3. 通用性

因為是利用程式語言透過 ORM 去對資料庫進行操作，只要使用 ORM 有支援的資料庫，就算更換成另一個資料庫也能夠讓程式成功執行，還能夠避免不同資料庫間的語法差異。

## Cons

### 1. 效能

畢竟ORM是將程式語言轉換成資料庫操作，本質上一定會比直接用資料庫語法查詢來的更花上時間。
### 2. 複雜查詢維護性低

在查詢比較複雜的狀況下，ORM內建的函式可能沒有辦法滿足我們的要求，這時就還是得利用原生資料庫語法去進行查詢。就會造成程式語言跟原生資料庫語法混雜的情況。

```ruby
User.where(“age between ? and ?”, 10, 30)
```
參考 : 
[後端工程師的第一堂課 (20) : 現代系統資料工具 — ORM. 這是篇共 30 篇的後端領域入門系列文章，預計 1 -2 週新增 1… | by Johnliutw | JohnLiu 的軟體工程思維 | Medium](https://medium.com/johnliu-%E7%9A%84%E8%BB%9F%E9%AB%94%E5%B7%A5%E7%A8%8B%E6%80%9D%E7%B6%AD/%E5%BE%8C%E7%AB%AF%E5%B7%A5%E7%A8%8B%E5%B8%AB%E7%9A%84%E7%AC%AC%E4%B8%80%E5%A0%82%E8%AA%B2-20-%E7%8F%BE%E4%BB%A3%E7%B3%BB%E7%B5%B1%E8%B3%87%E6%96%99%E5%B7%A5%E5%85%B7-orm-359da9a1d14a)
[什麼是 ORM ， 認識物件關聯對映 - HackMD](https://hackmd.io/@yoji/SJhowL8Ij?utm_source=preview-mode&utm_medium=rec)


