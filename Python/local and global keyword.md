---
date: 2023-03-15 Fri 23:45
---
---

## 函式中的變數

```python
id = 2
def func():
	id = 1
	
print(id)
```

上面這段程式執行的結果會印出的值為2。原因是因為當在python中宣告變數且沒有加上任何keyword的時候，函式內部定義的變數都會被視為==區域變數==。

以上面的情況來形容的話，雖然程式碼中具備id這個全域變數，但因為`func`中的id被視為區域變數，所以並不會去向外尋找id=2這個全域變數，而是會被視為區域變數。

而這也是為甚麼會有`global`與`local`keyword的原因。

透過在函式中的變數宣告前加上`global`

```python
id = 2 
func():
	global id
	id = 1
print(id) #1
```

就可以告訴函式我們所想要使用的變數是外部的全域變數，就能夠達成我們的目的

## global()與local()

+ global()
	+ 會回傳一個dictionary，其中包含所有全域變數
+ local()
	+ 會回傳一個dictionary，包含當前區域的區域變數。

## Reference
[全域變數、區域變數 - Python 教學 | STEAM 教育學習網 (oxxostudio.tw)](https://steam.oxxostudio.tw/category/python/basic/global-variable.html#google_vignette)