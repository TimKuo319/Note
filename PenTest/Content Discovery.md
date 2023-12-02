---
date : 2022-11-22 11:15
alias : []
last-modified : 2022-11-28 23:26
---

---

## Reflection
基本上都還是處在被動偵查的部份, 前面講是對一些尋常的網站設定做搜索及觀察屬於`Manually Discovery`, 像是robot.txt, Sitemap.xml, 還有可能因為使用框架卻`未更改favicon造成框架資訊外洩`, 這個部份可以將favicon經過`md5 hash`之後去`owasp favicon database`尋找, 可能可以獲得一些有用的資訊. 再來是藉由`OSINT`的方式, 即公開資源, 像是wappalyzer`可以用來分析網站所使用的技術, 如框架, cms,等 `, 還有github, amazon的s3bukets等等 , 都是可以利用osint.

在這個過程中也有利用到`curl`指令, 但我對於curl指令的用途目前還不太熟悉, 需要去熟練一下, 還有最後automated discovery的部份是利用了`ffuf, dirb, gobuster等工具`, 也是我自己沒有接觸過得部份, 也需要去實際操作一下才能對這些自動化工具更加了解.


2022-11-28更 :
ffuf, dirb, gobuster等工具原理`主要是利用字典檔對目標進行brute-force`,使用上多為用來列出`子網域, 網站`內目錄等等, 來增加資訊, 利於增加滲透測試的攻擊面. 但目前對於三者的差異感受還不是太明顯, 等到之後查完資料或使用有感後, 再回來補上這個部份


---

## Summary
這個部份主要是藉由不同的資料來源來找到更多關於網站的資訊, 對這些資訊更加了解的話, 將可以大幅增加滲透測試的效率, 因為對目標了解的越多, 所可能發現的漏洞也會越多.
