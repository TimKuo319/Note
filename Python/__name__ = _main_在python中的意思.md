---
date : 2023-05-06 22:23 Sat
alias : [_name_=_main_在python中的意思]
---

---

## 內建、隱含的變數

在python中有些內建、隱含的變數，而`___name__`就是其中之一，而`_name_`這個內建變數所代表的意義就是`模組名稱`，當某個檔案被引用的時候。他的模組名稱，也就是`___name__`就會是該檔案名稱。而該檔案若是被直接執行，則`__name__`則會是main

```python
# test.py

def foo():
	print("this is from fucntion foo")

print('Call it locally')
foo()
```

```python
# other.py
from test import foo

print('Call it remotely')
foo()
```

原先預期`other.py`的輸出會是兩行輸出，也就是`call it remotely`及`foo function`的內容，但實際上會印出四行，也就是原先`test.py`中的兩個print以及`other.py`中的兩個print，會造成這樣的原因是因為python檔案在被引用的時候，檔案內的`每一行都會被直譯器讀取且執行`，所以如果要避免執行的部分就可以使用`if __name__` 來避免原檔案的程式碼被執行。

參考:http://blog.castman.net/%E6%95%99%E5%AD%B8/2018/01/27/python-name-main.html