---
date: 2024-02-28 Wed 15:17
---
#Browser

---

## 同源政策

同源三條件:
+ protocol
+ domain name
+ port

阻擋來自非同源網站的回應。ex: 當A網站向B網站發出request的時候，因為不同源的關係，所以就算伺服器成功response，`瀏覽器也會擋下server response`。

## CORS(Cross-Origin Resources Sharing)

要取得不同網站的資源時，需要在server-side設定允許被該網站訪問，這樣瀏覽器才不會阻擋server的回應。

+ Simple request
	+ 在符合簡單請求的狀況下，瀏覽器會直接對server side發出請求，規範可見下方MDN doc。
+ Preflight request


# Reference

[跨來源資源共用（CORS） - HTTP | MDN (mozilla.org)](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/CORS)
[Preflight request - MDN Web Docs Glossary: Definitions of Web-related terms | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request)
[CORS 是什麼? 為什麼要有 CORS? ｜ExplainThis](https://www.explainthis.io/zh-hant/swe/what-is-cors)