---
date: 2024-04-30 23:54 Tue
---
---

## Decorator的用途

在python中，常常會看到function上方會有`@uppercase`，這類型at符號加上一個名字的語法，這個語法就稱作decorator。

透過為function加上一個裝飾器(decorator)，來去裝飾目標函式，讓他有不同的變化，這就是decorator存在的用意。

## How does it work

首先要提到的是`First-citizen`，一等公民是指該實體可以像變數一樣被賦值給別人，而在python中，function就是這樣的一個角色。也就是說，function可以`被作為變數或參數傳遞給別人`。

decorator的做法其實就是將目標函式作為參數，傳入其中，並回傳另外一個函式供使用。

以下程式碼引用自[Python進階技巧 (3) — 神奇又美好的 Decorator ，嗷嗚！ | by Jack Cheng | 整個程式都是我的咖啡館 | Medium](https://medium.com/citycoddee/python%E9%80%B2%E9%9A%8E%E6%8A%80%E5%B7%A7-3-%E7%A5%9E%E5%A5%87%E5%8F%88%E7%BE%8E%E5%A5%BD%E7%9A%84-decorator-%E5%97%B7%E5%97%9A-6559edc87bc0)
```python

def print_func_name(func):
	def wrap():
		print("Now use function '{}'".format(func.__name__))
		func()
	return wrap

@print_func_name
def dog_bark():
	print("Bark !!!")

@print_func_name
def cat_miaow():
	print("Miaow ~~~")

if __name__ == "__main__":
	dog_bark()
	# > Now use function 'dog_bark'|
	# > Bark !!!|

	cat_miaow()
	# > Now use function 'cat_miaow'|
	# > Miaow ~~~|
```

從上述程式碼可以看到，`print_func_name`作為decorator，其中包含了被傳入的函式，並且額外多印出了"Now use function..."這一行，透過`@print_func_name`，讓`dog_bark`以及`cat_meow`兩個函式有了新的樣子。

而會造成這樣的原因是因為，在main區域被呼叫的`dog_bark`以及`cat_meow`其實是`print_func_name`中回傳的`wrap 函式`。

## Types of decorator

上面使用到的decorator是以function的形式作為裝飾器，所以被稱作function decorator。而class也能夠作為裝飾器，則會被稱作class decorator。

與上述程式碼出自相同文章
```python
class Dog:
    def __init__(self, func):
        self.age = 10
        self.talent = func

    def bark(self):
        print("Bark !!!")

@Dog
def dog_can_pee():
    print("I can pee very hard......")

if __name__ == "__main__":
    dog = dog_can_pee

    print(dog.age)
    # > 10

    dog.bark()
    # > Bark !!!

    dog.talent()
    # > I can pee very hard......

```

這裡透過將class作為裝飾器，在function被傳入class時，透過constructor去將傳入的function作為該instance的talent函式。這樣的做法使多個實例可以`擁有不同的talent function`，而不需要去用到繼承或抽象的方式來達到，可以達到==簡化程式碼的目的==

當然還有許多decorator相關的用法，引用到的這篇文章還有介紹，之後有需要可以再重複翻閱

## Reference
[Python進階技巧 (3) — 神奇又美好的 Decorator ，嗷嗚！ | by Jack Cheng | 整個程式都是我的咖啡館 | Medium](https://medium.com/citycoddee/python%E9%80%B2%E9%9A%8E%E6%8A%80%E5%B7%A7-3-%E7%A5%9E%E5%A5%87%E5%8F%88%E7%BE%8E%E5%A5%BD%E7%9A%84-decorator-%E5%97%B7%E5%97%9A-6559edc87bc0)
