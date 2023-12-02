---
date : 2022-12-03 Sat 15:19
alias :[]
---

---

## Reflection
IDOR(Insecure Direct Object Reference)，這類問題通常發生在網站`直接`使用user的輸入去對資料進行存取，假設今天有一個url`http:example.com?user_id=123`，而網站直接依照使用者的輸入去參考其他資料的話，只要有惡意的使用者去修改url中user_id的部份，就可以繞過驗證，直接存去到其他使用者的頁面。

File Inclusion，又分為Local File Inclusion及Remote File Inclusion。
LFI最常見的方式通常是藉由`directory traversal`的方式來存取到機器內不同的資料，漏洞最主要的原因是`沒有對使用者輸入進行過濾`，因為在常見的php function中像是`include()`,`include_once()`,`require()`,`require_once()`,`fopen()`,`file_get_contents()`等等都會去參考機器內的檔案, 若是對user input沒有進行過濾的話，則hacker可以藉由這些function的特性來取得機器中重要的檔案。

RFI前提是php有開啟`allow_url_fopen`及`allow_url_include`(即是否允許包含外部url)
RFI的行為模式則通常是attacker讓target server去向一個外部的server(通常會是攻擊者的)發出request，取得惡意檔案，並且使得這個惡意檔案在target server中被執行，而因為attacker向taget server發出了這個請求，當taget server執行完後就會response到attcker那裡，造成資訊外洩。 

## Summary
在使用函式對機器內的資料進行存取時，應小心使用，並且對於使用者輸入進行過濾，預防惡意輸入造成的危害。

