---
date: 2024-03-21 21:56 Thu
---
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

## How JWT work

當用戶在請求受保護的資源的時，瀏覽器應該要在header中攜帶以下格式，來讓server從header去解析JWT，並決定是否允許用戶訪問這些位置
```texrt
Authorization: Bearer <token>
```

## Pros and Cons

+ Pros
	+  簡潔，便於傳輸
	+ 無狀態，相對session來說壓力更小，因為server端不需要去維護相關資料
+ Cons
	+ 無法回收，相對session能夠在server端設定過期或無效，JWT只能等到過期才會失效
	+ payload只做了`base64`編碼，如果被拿走的話，內容會直接曝光
	+ token如果被盜用的話，就會存在風險，因為在server端是`stateless的`


# Reference

[JSON Web Token Introduction - jwt.io](https://jwt.io/introduction)
[JWT 的運作原理是什麼？｜ExplainThis](https://www.explainthis.io/zh-hant/swe/jwt)