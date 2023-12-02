---
date : 2022-12-03 Sat 14:48
alias :[]
---

---

## Reflection

這個部份主要介紹的是繞過驗證的方式，前面task2跟3主要都是藉由`ffuf`用暴力破解的方式去列舉出網站的使用者帳號密碼，以下為常見參數:

1. `-w` : 代表要使用的wordlist的位置，ex:`usr/wordlist/name.txt`
2. `-x` : 代表要使用的http method , default為`GET`
3. `-d` : 代表要傳送出去的資料, ex:`"username=FUZZ&email=x&password=x"`，其中x為可任意填寫的部份，`FUZZ`則代表wordlist要替換的位置.
4. `-H` : 可用來修改http header， 例如要使用post method時，為了要讓server知道我們是要將資料傳送給他的，所以需要將header中的content-type改為`application/x-www-form-urlencoded` ex: `-H "Content-Type: application/x-www-form-urlencoded"`
5. `-u` : 目標網址
6. `-mr`: 列出符合正則表達式的response
7. `fc` : 過濾掉特定的status的 response

若有多個wordlist時，可以使用`,`來隔開， 並且簡化wordlist的代號。ex:```
``` sh
ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://MACHINE_IP/customers/login -fc 200
```

最後是藉由cookie來繞過驗證，因為現在在身份驗證上大多是使用session來進行驗證，若是將session存在cookie中，且沒有經過嚴謹的處理，使得使用者可以知道session處理的方式，便可能造成session被偽造，用戶身份遭到假冒。

## Summary
在現在許多的應用程式中，都會有需要使用者輸入帳號密碼來進行身分驗證的方式，在設計上需要足夠嚴謹，避免對使用者透漏太多有關系統本身的資訊，像是`"username exists"`這類的訊息。以防止惡意使用者找到漏洞進而造成危害。