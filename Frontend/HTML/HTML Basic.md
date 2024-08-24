
### HTML Basic
+ `<a>`
	+ href內可填#用來做佔位符或導向其他相同頁面的位置
+ Semantic tag - 用來表示語意的元素，方便做SEO以及維護
	+ `<header>` - 用於表示導覽或主要訊息
	+ `<main>` - 每個頁面只該出現一次，表示每一頁面的獨特訊息
	+ `<nav>` - 通常用於`header`中
	+ `<article>`
	+ `<section>` - 通常會搭配heading做使用，
	+ `<aside>` - 通常與主頁面沒有關係，向是廣告，側邊欄等等
	+ `<footer>` - 
+ `<blockqoute>`
### Image
+ `<img>`
	+ src - 圖片來源
	+ alt - 顯示不出來時顯示的文字
	+ title - 停留在圖片上時會出現說明 
+ `<figure>` - 搭配img做使用
```html
<figure>
	<img src="...", alt="...">
	<figcaption>描述圖片相關的訊息</figcaption>
</figure>
```

### Break

+ `<br>` - 換行
+ `<hr>` - 添加分隔線

## 改變字型

+ `<strong>`
+ `<em>`
+ `<small>`

## 外部連結

+ `<link>` - 用來連接CSS檔或JS檔
	+ rel屬性 - 檔案跟這個連結的關係
		+ stylesheet
		+ icon

## Inline element vs Block element

+ Inline element 
	+ 其中的內容會在同一行內
	+ `<span>` - 可用於在行內調整字型的外觀
	+ `<strong>`
	+ `<em>`
+ Block element
	+ 其中的內容之間通常會有break(換行)
	+ `<div>`

## id

+ 可以在element中作為屬性，表示獨特的值
+ 在其他連結中指定ID時，透過`#id`的方式。

# Link to email 

```html
<a href="mailto:your email">....</a>
```

With Subject :

```html
<a href="mailto:your email?subject=....">....</a>
```

若是subject中使用到空白，用url enconding的方式顯示會比較穩定
+ 假設句子是"Hi there!" -> "Hi%20there!"

## HTML Entities and Reserved Characters

有些包含在html文法中的符號或文字需要使用時，需要以其他的形式寫在html中