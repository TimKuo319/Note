

## var、let 、const 差異

+ var
	+ function scope
	+ 可以重複宣告相同的變數
	+ 當不在function內進行宣告時，會變為全域變數
	+ Hoisting[^1]的時候，值會被初始化為`undefined`
+ let 
	+ block scope
	+ 在同一個block中重複進行宣告時，會造成syntax error
	+ Hoisting的時候，值不會被初始化，造成reference error
+ const
	+ block scope
	+ 在宣告const變數時`必須`進行初始化
	+ 宣告後，值即不可更改
	+ Hoisting的時候，值不會被初始化，造成reference error

## 為何const宣告物件時，能夠改變物件內的值

+ 對於像是Object、Array、Function等(Obejct Type)
	+ 在賦值或傳遞的時候都是透過reference的方式進行

+ 因為是改變物件內部的屬性關係，並不會影響到物件原先的reference，所以這樣的改動在const宣告下是合理的

+ by reference vs by value
	+ 前面提到的`Object、Array、Function`等等
		+ call by reference
	+ primitive type
		+ `undefined、null、boolean、number、string、symbol`
		+ call by value
		+ 值跟值之間是獨立的
			+ 比如說兩個數字1都被賦予到新的變數，而這兩個1是兩個獨立的個體


## Data Manipulation

+ `in`跟`of`的區別
	+ in用於確認物件內是否具有該屬性
	+ of用於走訪陣列、map、string等等
+ `.` vs `[]`進行物件值取得
	+ 利用`.`時，只能指定已經確定存在於object中的屬性
		+ 只能進行`靜態訪問`，也就是先指定好屬性名稱
	+ 利用`[]`時，key可以是一個變數或表達式，且內容可以是`任意字串`
		+ 可以進行`動態訪問`，也就是其中可以放入變數等等，相對靈活

| 特性        | obj.key               | obj[key]             |
| --------- | --------------------- | -------------------- |
| 語法        | 使用點號（.）               | 使用方括號（[]）            |
| 屬性名類型     | 必須是有效的 JavaScript 標識符 | 可以是任何字符串或數字          |
| 動態訪問      | 不支持                   | 支持                   |
| 變量作為屬性名   | 不支持                   | 支持                   |
| 表達式作為屬性名  | 不支持                   | 支持                   |
| 特殊字符在屬性名中 | 不允許                   | 允許                   |
| 數字作為屬性名   | 不直接支持                 | 支持（會被轉換為字符串）         |
| 在循環中使用    | 較少用                   | 常用（如 for...in 循環）    |
| 程式碼可讀性    | 直觀，適合已知屬性名            | 靈活，適合動態情況            |
| 性能        | 略快（靜態解析）              | 略慢（動態解析）             |
| 使用場景      | 固定的、已知的屬性名            | 動態決定的屬性名，或包含特殊字符的屬性名 |

## Arrays

+ join() - 將array內的所有值匯聚成一個字串
+ include() - 檢查元素是否存在陣列中
+ filter() - 透過==shallow copy==的方式建立一個array，並將後方符合條件的值留下
+ reduce()
```js
const numbers = [1, 2, 3, 4, 5];
const initialValue = 10;
const sum = numbers.reduce((accumulator, currentValue) => {
  return accumulator + currentValue;
}, initialValue);

console.log(sum); // 输出 25，因为 10 + 1 + 2 + 3 + 4 + 5 = 25


```

## DOM

+ 瀏覽器具有一個global object，稱為`window`
	+ 其中包含許多不同的屬性
		+ alert
		+ blub
		+ atob
		+ ...


### document vs Document

+ document通常是window.document的縮寫，用來操作當前的載入的網頁
+ Document則是一個Interface，其中包含了許多method跟property
	+ 有額外用途需要自己擴充Document中的method時可使用


### Different Selection Method

+ getElementById() - `透過id取得特定元素`
	+ param - id

+ getElementsByTagName() - `透過tagName取得元素`
	+ param - tagName
	+ 回傳一個`HTMLCollection`(類陣列結構)
	
+ getElementsByClassName() - `透過className取得元素`
	+ param - className
	+ ==回傳一個HTMLCollection==

+ 以下兩者皆能夠存取各類型的selector，ex:`id, class ,tag`
	+ querySelector()
		+ 存取第一個符合條件找到的元素
	+ querySelectorAll()
		+ 存取所有符合條件的元素
		+ ==回傳`NodeList`(另外一種類陣列結構)==

### NodeList vs HTMLCollection

+ Nodelist
	+ static datastructure
+ HTMLCollection
	+ live datastructure

### 改變HTML element 內容

+ textContent
	+ 將所有element的內容視為文字內容
+ innerHTML
	+ 能夠解析HTML內容，可在element內創造更複雜的HTML結構


### Change Element Attribute

+ 利用`.`方式來存取element的屬性
```js
const headline = document.getElementById('headline');
headline.title = '...'
```

+ 對於class屬性，在利用js存取的時候，需要改為`className`
```js
const headline = document.getElementByid('headline');

//將headline的className更改為test
headilne.className = 'test'
```

### Set Inline Styles

+ 與前面存取屬性的方式相同，只是style property會回傳所有可用的CSS屬性

### Create、Append、Remove DOM Elements

+ document.createElement()
	+ 建立element

+ ParentNode.append()
	+ param - child node
```js
const list = document.querySelector('ul');
const item = document.createElement('li');
//將item加入到DOM中
list.append(item);
```

+ ParentNode.prepend()
	+ 將childNode加到所有childNode最前端

+ element.insertAdjacentHTML(positoin, htmlElement)
	+ 將html元素放到特定位置

+ node.remove()
	+ 將element從DOM中移除


### Interacting with DOM

+ event delegation
+ selector.child - 得到該元素所有子元素
+ selector.parentNode - 得到該元素的父元素

## forEach vs map

+ 兩種都是迭代陣列的方式
+ forEach
	+ 沒有回傳值
	+ 通常用來作為外部操作或修改原陣列
+ map
	+ 會回傳一個新的陣列
	+ 通常用來創建一個新陣列

[^1]: : 在JavaScript中，當一個變數或function在宣告前就被使用就稱作hoisting，不會出錯的原因是因為，JavaScript在執行前，會先掃描過所有程式碼，他會先將變數或function的宣告先放到記憶體中，並依照宣告性質給予不同的初始值(var是undefined，let、const沒有初始值(`會產生error`))，再去執行。