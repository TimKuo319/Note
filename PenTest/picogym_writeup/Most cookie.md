---
date : 2022-11-29 18:28
alias :[]
---

因為前面有類似的cookie題, 所以這一題一開始就猜測大概是以換cookie的方向為主, 但點開之後發現, cookie不是像之前一樣單純的一個數字, 而是一串混雜的字串, 在看到提示說了有關flask的安全後, 就去了解一下flask seesion, 發現他是一種將狀態儲存在用戶端的client side cookie, 而flask seesion 大致結構為, `data.timestamp.sign`data為經過base64encode的字串但去掉填充的等號, 中間為時間戳記, 最後的sign為將sessiondata timestamp與flask secret key經過sha1運算過得結果. 當server端收到cookie的時候, 就會將前面的data與timestamp拿出來跟secret key做sha1運算, 當與第三段結果不同時, 則cookie無效.也因此, 若有人從客戶端修改cookie, 傳回server時會造成sign驗證不過, cookie無效. 從上可知, secret key是很重要的, 而這題剛好有給我們`server.py的source`, 從中可以看到secret key的list, 代表secret key肯定是其中一個, 那這個時候我們再用flask unsign, 先用`flask-unsign --cookie --decode '{$cookie}'`來看一下data的部份, 會發現是`{very_auth : blank}`, 而依照`server.py`, 當very_auth為admin時, 就登入成功. 那接下來就是爆破secret key, 用`flask-unsign --unsign --cookie {$cookie} -w wordlist`. 得到secret key後, 再使用`flask-unsign --sign {'very_auth:admin'} --secret {$secret key}`就可以簽出認證的cookie, 修改cookie值, reload網頁後就會出現flag了.