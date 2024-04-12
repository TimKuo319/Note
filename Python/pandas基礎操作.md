---
date: 2024-03-30 12:55 Sat
---
---
# pandas 基礎

## 資料型態

pandas中主要有兩種核心的資料類型，Dataframe以及Series

+ DataFrame - 是一種二維的資料結構，類似一個table或excel試算表
	+ 可容納不同的資料型態(integers、floats、strings)
	+ 透過row跟column的label，可以輕鬆存取資料
	+ 支援各種不同的資料輸入方式，`csv, dictionary、資料庫查詢結果`等等
	
+ Series - 是一種一維的資料結構，類似python的list或numpy的一維陣列
	+ Series中的資料型態必須相同
	+ 可透過`labels`或`postion`來存取資料

## 常見使用函數

+ `pd.read_csv()` - 讀取csv檔案
+ `df.head()`/`df.tail()` - 取得dataframe中的前幾行或後幾行，參數為行數
+ `df.info()` - 取得dataframe的結構，像是欄位名稱、筆數、資料型態等等
+ `df.loc()`/`df.iloc()` - 取得資料位置
	+ `df[要取得的row分段, 要取得的column分段]`
	+ loc可以透過傳入column或row的label(可以是字串)，來取得資料
	+ iloc只能透過column或row的index(數字)來取得資料	

```python
import pandas as pd

data = {'名字': ['John', 'Amy', 'Bob', 'Kate'],
        '年齡': [25, 28, 32, 30],
        '城市': ['紐約', '芝加哥', '舊金山', '波士頓']}
df = pd.DataFrame(data)

# 選取第二個row
print(df.loc[2])  # 輸出: 名字 Bob, 年齡 32, 城市 舊金山

# 選取1,2,3row
print(df.loc[1:3])

# 選取1,2,3 row , 2,3,4 column
print(df.loc[1:3, 2:4])
```

+ `df.dropna()`/`df.fillna()` - 刪除/填補nan的值
+ `df.groupby()` - 根據條件對資料分組
+ `df.apply()` - 對每個元素應用函數
+ `df.merge()`/`df.join()` - 合併多個dataframe