---
date: 2024-03-21 21:56 Thu
last-modified: 2024-04-03 20:48 Wed
---
## JWT (json web token)

JWT是一種開放標準，利用簡單穩定的方式來進行身分驗證和數據授權。主要分為三個部分，會以`.`隔開

+ Header
+ Payload
+ Signature

所以JWT的格式通常會長得像以下這樣
```text
xxxxx.yyyyy.zzzzz
```


## Header

Header通常包含兩個部分
+ alg - alogrithm
	+ 決定簽章時要用的加密演算法
+ typ - type of token
	+ token的類型，通常是`JWT`

而header就會透過`base64`編碼轉換為token中第一段的內容。

## Payload

Payload通常就是我們要傳遞的訊息內容，內容主要是claim(聲明)，這些claim就是一些有關使用者的資料或額外的資訊，主要有三種類型的claim。

+ Registered claims 
	+ 一些建議填寫的相關資訊
	+ iss(issuer) - 發行者
	+ sub(subject) - token面向的用戶
	+ aud(audience) - token接收方
	+ [others](https://datatracker.ietf.org/doc/html/rfc7519#section-4.1)
+ Public claims
	+ 可以被公開定義的聲明,需要在JWT的發行者和接收者之間定義明確的語義,以便雙方能夠理解。一般很少使用。
+ Private claims
	+ 根據需求傳遞所需的數據，ex: userid、權限等等
	+ key的名稱可自定義，但必須要避開==保留字==

## Signature

簽章，這個部分就是用來驗證我們的token有沒有被竄改過的部分。在JWT的簽章部分，是透過將前面被`base64`轉換過的header以及payload，加上一個`secret`，再搭配前面在`header中指定的加密演算法`來完成

```text
HMACSHA256( base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)
```

## Step by Step

### 假設數據

1. Header
```json
{ 
  "alg": "HS256",
  "typ": "JWT" 
}
```

經過 `base64`

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

2. Payload

```json
{
  "user_id": 123,
  "role": "user"
}

```

經過 `base64`

```
eyJ1c2VyX2lkIjoxMjMsInJvbGUiOiJ1c2VyIn0
```

3. Secret
```json
my_secret_key
```


### 產生 token

header 經過 base64

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

payload 經過 base64

```
eyJ1c2VyX2lkIjoxMjMsInJvbGUiOiJ1c2VyIn0
```

signature 產生

```
HMACSHA256(eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9 + "." + eyJ1c2VyX2lkIjoxMjMsInJvbGUiOiJ1c2VyIn0 + my_secret_key)

// 假設生成結果
abc123xyz
```


最終的 Jwt

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoxMjMsInJvbGUiOiJ1c2VyIn0.abc123xyz
```

## How JWT work

當用戶在請求受保護的資源的時，瀏覽器應該要在header中攜帶以下格式，來讓server從header去解析JWT，並決定是否允許用戶訪問這些位置
```text
Authorization: Bearer <token>
```


## JWT vs session

### JWT
#### Pros:
+  速度相對session cookie快，因為資料被包含在token中，直接透過server解析，而不用進行大量查找。
+ Without sessionStore the same token is used to authenticate on different server as well, as long as the server have the SECRET key.
#### Cons:
+  沒有辦法主動被銷毀，只有當token設定的時間過期後才會無法使用，需要server端採取其他手段來確保該token不會被誤用。
+  Anyone able to perform a man-in-middle attack and sniff the JWT and authenticate the individual credentials because JWT is rarely encrypted.
+  隨著現在要儲存的資料變多，jwt可能會==超過url的長度==或==超過cookie能儲存的長度==而產生其他問題，且大量資料儲存在jwt中的話，在伺服器端解碼也需要相對長的時間。
#### Usage
+ Token生命週期較短的時 : 讓擁有此 Token 的用戶能夠在時間以內完成某些操作（e.g. 登入、下載檔案等）
+ 讓Token僅能被單次使用 : 避免jwt需要等到過期才報廢的狀況

---
### Session
#### Pros: 
+  可以主動的操控session過期的時間 -> 直接從server端撤銷seesion
#### Cons:
+  因為需要在server端儲存使用者的狀態，且在使用者每一次使用者的請求中都要透過session_id查找對應資料，當數量龐大可能會造成response time變長。
+  可能會被XSS
+  因為是在server端維護session，當伺服器出現其他安全問題時，也可能因此受到破壞
#### Usage
+ 身分驗證
+ 客戶端狀態儲存
---

# Reference

[JSON Web Token Introduction - jwt.io](https://jwt.io/introduction)
[JWT 的運作原理是什麼？｜ExplainThis](https://www.explainthis.io/zh-hant/swe/jwt)
[Effective Ways to Authenticate Users (halodoc.io)](https://blogs.halodoc.io/user-authentication-jwt-vs-session/)
[淺談 Session 與 JWT 差異. 介紹 Session 與 JWT 的使用時機等等 | by 集點送紅利 / Hiro | Medium](https://medium.com/@jedy05097952/%E6%B7%BA%E8%AB%87-session-%E8%88%87-jwt-%E5%B7%AE%E7%95%B0-8d00b2396115)
[JSON Web Token 與 Session 差別 ?. 是不是快忘記了 ? 我是有點突然不知道怎解釋，所以來筆記紀錄一下… | by 小路 | 從心開始 | Medium](https://medium.com/%E5%BE%9E%E5%BF%83%E9%96%8B%E5%A7%8B/json-web-token-%E8%88%87-session-%E5%B7%AE%E5%88%A5-468492b18fd0)
[JWT 的運作原理是什麼？｜ExplainThis](https://www.explainthis.io/zh-hant/swe/jwt)