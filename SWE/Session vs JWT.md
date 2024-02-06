---
date: 2024-02-06 Tue 16:12
---
#Authentication 

---

# JWT
## Pros:
+  速度相對session cookie快，因為資料被包含在token中，直接透過server解析，而不用進行大量查找。
+ Without sessionStore the same token is used to authenticate on different server as well, as long as the server have the SECRET key.
## Cons:
+  沒有辦法主動被銷毀，只有當token設定的時間過期後才會無法使用，需要server端採取其他手段來確保該token不會被誤用。
+  Anyone able to perform a man-in-middle attack and sniff the JWT and authenticate the individual credentials because JWT is rarely encrypted.
+  隨著現在要儲存的資料變多，jwt可能會==超過url的長度==或==超過cookie能儲存的長度==而產生其他問題，且大量資料儲存在jwt中的話，在伺服器端解碼也需要相對長的時間。
## Usage
+ Token生命週期較短的時 : 讓擁有此 Token 的用戶能夠在時間以內完成某些操作（e.g. 登入、下載檔案等）
+ 讓Token僅能被單次使用 : 避免jwt需要等到過期才報廢的狀況

---
# Session
## Pros: 
+  可以主動的操控session過期的時間 -> 直接從server端撤銷seesion
## Cons:
+  因為需要在server端儲存使用者的狀態，且在使用者每一次使用者的請求中都要透過session_id查找對應資料，當數量龐大可能會造成response time變長。
+  可能會被XSS
+  因為是在server端維護session，當伺服器出現其他安全問題時，也可能因此受到破壞
## Usage
+ 身分驗證
+ 客戶端狀態儲存
---
# Reference

[Effective Ways to Authenticate Users (halodoc.io)](https://blogs.halodoc.io/user-authentication-jwt-vs-session/)
[淺談 Session 與 JWT 差異. 介紹 Session 與 JWT 的使用時機等等 | by 集點送紅利 / Hiro | Medium](https://medium.com/@jedy05097952/%E6%B7%BA%E8%AB%87-session-%E8%88%87-jwt-%E5%B7%AE%E7%95%B0-8d00b2396115)
[JSON Web Token 與 Session 差別 ?. 是不是快忘記了 ? 我是有點突然不知道怎解釋，所以來筆記紀錄一下… | by 小路 | 從心開始 | Medium](https://medium.com/%E5%BE%9E%E5%BF%83%E9%96%8B%E5%A7%8B/json-web-token-%E8%88%87-session-%E5%B7%AE%E5%88%A5-468492b18fd0)
[JWT 的運作原理是什麼？｜ExplainThis](https://www.explainthis.io/zh-hant/swe/jwt)