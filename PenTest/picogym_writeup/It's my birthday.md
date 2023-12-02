---
date : 2022-11-26 Sat 19:52
alias :[]
---

題目說送出的兩封邀請函有相同的md5 hash , 且可以讓我們上傳兩個檔案.
自己最一開始的思路就是去找md5 collision也成功在網路上找到相關的文章 , 最後從裡面下載了兩個md5 collision的檔案並更改檔名為pdf後上傳就拿到flag了.

在去看了別人的write up之後發現, 比較偏向intended solution的解法應該是利用php語言上的漏洞`Magic hash`. 這個漏洞原因是php在處理 hash字串時, 會利用`!=`及`==`來對hash value做比較, 而php會將每一個以0e開頭的hash value解釋為0, 因此如果有兩個hash過的值開頭為0e的話, 不論0e後方的值`相同與否`, php會將`兩者視為相等的`. 而從upload檔案之後, 可以從網址列看到其實頁面是用php寫的, 因此我們就可以利用這個漏洞來上傳兩個開頭為0e的hash值, 讓網站判斷兩者皆為0 , 最後得到flag