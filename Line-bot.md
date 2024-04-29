---
date: 2024-04-29 21:01 Mon
---
---

## Register

要利用line-bot，首先要去註冊`developer`帳號，[Log in to LINE Developers | LINE Developers](https://developers.line.biz/en/docs/line-developers-console/login-account/)，接著需要去註冊provider(*註1*)以及channel(註2)，channel就會是我們之後啟用的line-bot，所以channel的相關資訊就會是之後在line上面看到的資訊。

## Mode of line-bot

line-bot 主要會有兩種模式，auto-response以及webhook

+ Auto-response
	+ Auto-response模式可以透過以下畫面，點擊Auto-reply messages去設定line-bot要自動回覆的訊息
	+ 路徑: `your channel > Messageing API`接著往下滑就會看到
	+ ![line_bot](../image/line_bot_auto_response.png)
+ webhook
	+ webhook模式則是可以讓我們自己撰寫程式，透過替line-bot建立一個`messaging server`來設定當接收到某些訊息的時候，server可以怎麼去回應客戶端
	+ 以下是`messaging API的運作模式`
	+ ![message_api](../image/messaging-api-architecture.png)
	+ [Messaging API overview | LINE Developers](https://developers.line.biz/en/docs/messaging-api/overview/#how-messaging-api-works)

## Building line-bot

首先要先建立我們messaging server，因為line官方本身就有推出，[line-bot-sdk-python](https://github.com/line/line-bot-sdk-python)，所以可以透過這個套件去撰寫我們的messaging server，而其中因為有使用到flask這個python framework，所以就需要安裝這兩個套件。
```python
pip install line-bot-sdk
pip install flask
```

整個line-bot的運作過程
1. line-bot遇上各種事件(接收訊息、加入群組、有人退出群組等等)
2. 將這個event傳給`line platform`
3. `line platform`依照line-bot填寫的webhook URL，傳遞request到messaging server。
4. messaging server依照各種不同的event進行response，`line platform`再將response回應給使用者

(尚未熟悉line-bot-sdk使用，todo)
## Reference

[Messaging API overview | LINE Developers](https://developers.line.biz/en/docs/messaging-api/overview/#how-messaging-api-works)
[Day 28 : 撰寫LineBot，利用短短三天認識自動化機器人(中) - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天 (ithome.com.tw)](https://ithelp.ithome.com.tw/articles/10280447)
[line/line-bot-sdk-python: LINE Messaging API SDK for Python (github.com)](https://github.com/line/line-bot-sdk-python)
