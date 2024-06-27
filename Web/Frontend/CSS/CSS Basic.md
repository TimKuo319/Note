
## 相關資源

+ MDN
+ CSS Tricks
+ Can I use
+ W3C
## Inline Style

+ 直接在html元素內利用css屬性
+ css層級運用最大的style
	+ 後續使用internal style或external style都無法覆蓋掉inline style

## Internal Style

+ 通常在會在head中使用，以`<style></style>`將要指定的元素與使用的樣式包含在其中

## External Style

+ 將CSS額外整理到一個檔案中撰寫，可以方便在同一檔案中修改CSS樣是即可
+ 在瀏覽器回應請求時，CSS檔案第一次被請求後，就會被瀏覽器cache住，可以具備更好的網路效能


## 撰寫格式

```css
selector {
	property: value;
}
```


## ID Selector 、 Class Selector、Attribute Selector

+ ID selector - 適用於目標元素只有單一元素時
	+ 以`#`開頭
+ Class selector - 適用於目標元素不只一個元素，但也非全部元素的時候
	+ 以`.`開頭
```css
/*段落為p，且class為heading*/

p.heading{
	property: value
}
```


+ Attribute Selectors
	+ 用`[]`將屬性包裹起來
	+ 針對有...屬性的element
	+ 以下面的例子來說，就是針對有title這個屬性的元素，將color設為`indigo`

```css
[title] {
	color: indigo;
}
```

+ ID權重大於Class權重
	+ 有相同屬性時，id的樣式會覆寫class樣式


## Descendant Selectors

```css
first selector second selector target selector{
	property: value;
}
```

+ Father selector也可以是id或class

## Target Direct Children

+ 利用`>`符號來指向該元素直接的子元素
```css
ul > li {
	border: 1px dashed lightgrey;
}
```

## Target Adjacent Siblings

+ 用來指向`向下`相鄰在該元素旁邊的元素
```css
img + p {
	margin-bottom: 20px;
}
```


## Target General Siblings

+ 用來指向向下的所有元素
+ 以下面的例子來說，指的就是header往下，所有的div都會變成`aliceblue`

```css
header ~ div {
	background: aliceblue;
}
```


## Target a  Child Element

+ 透過添加`first/last-child`關鍵字
	+ 指定第一個跟最後一個子元素

```css
li:first-child{
	...
}
```

+ 奇偶數子元素，在nth-child的括號內填even或odd
+ 特定子元素，在括號內填特定樹字
```css
li:nth-child(even) {
	color: white;
	background-color: springgreen;
}
```


## Naming Convention

+ BEM naming convetion

## Pseudo-classes

+ 以`:`開頭
+ 用來針對使用者行為進行不同的樣式使用
	+ visited
	+ focus
	+ hover
	+ active
	+ link
		+ 還沒被訪問的時候
+ 針對結構元素
	+ first-child
	+ last-child


## CSS data types

+ 將一些同類型的屬型值整合為一個種類(datatype)，供property使用
	+ 長度
		+ em
		+ px
		+ 
	+ 顏色值
		+ 英文字
		+ 色號

### Length Unit

+ Absolute length unit
	+ px

+ Relative length unit
	+ percentage
		+ ==相對於parent element的長度==
	+ 常被用於字體大小的unit
		+ em
			+ 相對於父元素字型大小計算
			+ 會逐層疊加
		+ rem
			+ 相對於根元素(html)計算
			+ 不受父元素影響，僅計算一次

+ 瀏覽器預設的字體大小為16px
	+ 在設定html元素時，可以將font-size設為`1em`或`100%`，這樣當使用者調整字型大小時，才可以跟著改變

### Color Values

+ 三種表示顏色的方式
	+ 16進制表示
		+ 像是cc7733這類型的顏色可縮短為c73
			+ 因為各顏色位置代表的字都重複
	+ 顏色名
	+ rgb(red, green, blue, alpha)



## 瀏覽器解決衝突的三種方式

1. UserAgent Style Sheet
2. User Style Sheet
3. Author Style Sheet

權重優先: 3 > 2 > 1

### Selector Specificity

+ id selector > class selector

### Source order

+ 後面的規則會覆寫掉前面的

+ 瀏覽器解決衝突的順序
	1.  Origin
	2.  Selector Specificity
	3. Source order

## Inherited property

+ 有些屬性會從parent裡copy過來 
+ https://developer.mozilla.org/en-US/docs/Web/CSS/inheritance

## Important Resources

+ W3C Validator 


## CSS Box Model

+ 由內到外
	+ content
	+ padding
		+ content跟border之間的距離
		+ padding 屬性順序
			+ 上右下左
	+ border
		+ border-style
			+ 顯示border樣式的屬性
	+ margin
		+ 跟別的元素之間的距離
		+ 可以有負值
		+ 水平置中時可以使用`auto`
		+ `auto`
			+ 將左右兩邊的寬度設為相同
			+ 在flex-item中使用的話，flex-item的main-axis及cross-axis都會自動置中
+ 針對多個selector需要使用相同屬性時，只要用`,`隔開即可

### box-sizing

+ default的情況，width跟heigth都僅限於content的內容，
+ border-box
	+ 在計算box-model的時候，會將padding與border計算進去

### max-width

+ 指定最大寬度

### Viewport Units

+ vw(viewport width)
	+ 1vw等於可視寬度的1%
+ vh(viewport heigth)


## CSS Layout

### Display

+ none
+ block
	+ 預設會佔滿parent container的所有寬度
+ inline
	+ 會跟其他元素保持在同一行中
+ inline-block
	+ 以上兩者的結合
	+ 元素一樣會在同一行間，不過可以進行寬度調整

### Relative and Absolute Positioning

+ relative
+ absolute
	+ 在整個頁面中的絕對位置
+ fixed
	+ 固定在可視畫面中
+ sticky
	+ 元素可以先在頁面的某個位置，當超過該位置後，元素會一直出現在可視畫面中。直到回復到他先前的位置
+ float
	+ 文繞圖時可以使用

### Clear

+ 在使用浮動元素後，可以透過clear來確保浮動元素旁的元素不受影響


### Media Queries

+ Anatomy of a Media Query
+ 指定在特定裝置上看起來的樣子

```css
@media print (min-width:400px){
	...
}
```

## Meta Viewport Declaration

+ 指定在網頁剛載入時，頁面的寬度或縮放該如何被解釋
+ `device-width`寬度設為跟裝置寬度相同
+ `initial-scale`，設定初始縮放比例，1代表不放大也不縮小

```html
<head>
	<meta name="viewport" content="width=device-width, initial-scale=1"
</head>
```

## Flexbox layout

+ Flex Container
	+ 包含flex items，以及實際應用flexbox的元素
	+ 可以是block或是inline元素
+ Flex Items
	+ Direct child of flex container
	+ 任意數量
+ Axis
	+ Main axis
		+ 主要排列flex item的方向
	+ Cross axis
		+ 垂直於main axis
+ Resources
	+ CSS-TRICKS

### Inline-flex

+ 將flex-container大小限縮至能滿足flex-item擺放的最小程度。

### Controlling the Direction of Flex Items

+ flex-direction
	+ 作用於flex-item
	+ `row`
	+ `column`

### Wrapping Flex Items

+ flex-wrap
	+ `wrap`
	+ 當container寬度不足時，會自動將flex-item換行，以讓flex-item看起來保留在flex-container內


### Distributing Space Inside a Flex Container

+ justifiy-content
	+ flex-end
	+ center
	+ space-between
		+ 頭尾的flex-item會放到container邊界
	+ space-around
		+  頭尾的flex-item與container間還是有距離

### Change the Order of Flex Items

+ order
	+ 設定數值，初始為0。所以如果設定數值為負的話就代表會成為最前面的flex-item
	+ 數值越大越後面，數值越小越前面


### Growing Flex Items

+ flex-grow
	+ 預設值為0(不會擴展)
	+ 相對於其他flex-item的倍數
		+ 2代表該item會是其他flex-item的兩倍寬


### flex-basis and flex

+ flex-basis
	+ 設定完值之後，當寬度大於flex-basis的寬度時，則會自動將flex-item分配為同樣寬度大小，小於時，則會被自動重新分配空間
+ flex以下三種屬性組合的簡寫
	+ flex-grow
		+ 僅容器空間有多餘時才會使用
		+ 放大比例
		+ 數字越大代表放大比例越多
	+ flex-shrink
		+ 僅容器空間有不足時才會使用
		+ 縮小比例
		+ 數字越大代表縮小比例越多
	+ flex-basis


### Align Flex Items on the Corss Axis

+ align-items
	+ 只適用於flex-items
	+ stretch - 預設值
		+ 會延伸到`填滿`flex container
	+ flex-start
	+ flex-end
	+ center
+ align-self
	+ 針對單一元素進行flex布局

### Vertical and Horizontal Centering

+ justify-content
	+ main-axis
	+ center
+ align-item
	+ cross-axis
	+ center
+ align-self
	+ cross-axis
	+ center
+ margin
	+ 皆可
	+ auto




## orientation

+ 螢幕擺放方向
	+ landscape
		+ 橫向模式
	+ potrait
		+ 直向模式

