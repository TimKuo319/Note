---
tags:
  - Network
---

## What is `Webhook`, why we need it

Webhook 是一種概念，主要是實現**事件驅動通知機制**的方式。在技術實作上，Webhook 其實就是一個 HTTP request，只是這個 request 是在特定的事件被觸發的時候才會被送出。

從上面的機制就可以發現，Webhook 其實就是將在一個系統上發生的某個事件的資訊，傳遞到另外一個系統上讓它依照這些資訊去做後續的使用。

在現實世界中，Webhook 常出現在像是 Saas、Paas 等服務，像是 GitHub 可以在 repo 中有 push 到 main 的時候透過設定 webhook URL 去通知 JenKins，或是像銀行帳號刷卡付費時，銀行也會有即時的通知傳遞到個人裝置上等等。

## Reference

- [What Are Webhooks And How Do They Work](https://hookdeck.com/webhooks/guides/what-are-webhooks-how-they-work)