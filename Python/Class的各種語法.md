---
date: 2024-04-30 22:45 Tue
---
---

## Constructor

python中的constructor是寫法是`__init__`，每當有要透過class來初始化instance的時候就會先去呼叫constructor

```python
class Person:
	def __init__(self, name, age):
		self.name = name
		self.age = age
```

## Double underscore(雙底線變數或方法)

Double underscore的變數或方法是python內建的，通常會具有一些特殊作用，更多的double underscore可以到python文件中去查詢

+ `__self__`
	+ 指向instance本身
+ `__class__`
	+ 指向class本身
+ `__len__`
+ `__setitem__`


## Three types of method of class

```python
class MyClass:
	class_attr = 0

	def __init__(self,name):
		self.name = name

	def run():
		print("I'm running")

	@classmethod
	def walk():
		print("I'm walking")

	@staticmethod
	def static_method(x,y):
		print(f'Result is: {x+y}')
```

+ Instance method
	+ class中沒有加上`decorator`的method，基本上就是instance method
	+ 第一個參數通常會是`self`(約定俗成的名稱)，`指向instance本身`
	+ intance method==僅能==存取instance attribute
	+ 透過instance去呼叫
+ Class method
	+ 加上`@classmethod` decorator的method
	+ 第一個參數通常會是`cls`(同樣是約定俗成的名稱), `指向class本身`
	+ ==僅能==存取class attribute，對應到上方程式碼的`class_attr`
	+ 透過`className.classMethod方式呼叫`
	+ 所有instance共享
+ Static method
	+ 加上`@staticmethod` decorator的method
	+ 本身作為組織目的，不與instance或class有相關聯
	+ ==無法==存取class attribute或instance attribute


## Property

在OOP的狀況下，我們通常會使用setter以及getter去更安全的存取class或instance的屬性，而python中內建提供我們方便的語法糖來讓setter以及getter被達成的同時，也讓程式碼更加簡潔

```python
class Person:
	def __init__(self,age):
		self.age = age
	@propery
	def age(self):
		return self.age

	@age.setter
	def age(self,value):
		if(value > 18):
			print("you are a adult")
```


### getter
property的使用方式很簡單，首先是在getter上方加上`@property`這個decorator，而function名稱可以取為屬性的名稱，以上面程式碼來說就是`age`，

### setter
在有了property後，我們就在setter的上方加上`@propertyName.setter`，並將setter function的名稱改為與getter名稱相同，這樣setter與getter的設定就完成了


這樣一來，當instance要去取得或改變age的時候，python就會自動替我們去呼叫setter以及getter的函式，來確保屬性是在我們的可控範圍下被存取的。