---
date: 2024-03-07 22:41 Thu
---

- [ ] 說明補充
## Prototype

+ 所有function都會有一個prototype屬性指向一個特殊物件==prototype==
+ ==prototype==物件中會有一個constructor屬性指向原來的function
+ 所有物件的有`__proto__`屬性，指向他繼承而來的`prototype`物件
+ 所有prototype chain的源頭是Object.Prototype，所以Object.Prototype的`__proto__`是null


## Reference

[原型基礎物件導向 · 從ES6開始的JavaScript學習生活 (gitbooks.io)](https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part4/prototype.html)