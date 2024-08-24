
### 透過CDN載入React Libray

```html
<!DOCTYPE html>

<html>

  <head>

    <meta charset="UTF-8" />

    <title>My React App</title>

    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>

    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>

    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

  </head>

  <body>

    <div id="test"></div>

    <script type="text/babel" src="app.js"></script>

  </body>

</html>
```

以上方程式碼來說，透過在head中先載入React，來供後續我們使用JavaScript時能夠使用到React的功能。

## Key Part

+ `<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>`
	+ 用途 -  ==使用React來建立使用者介面==

+  `<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>`
	+ 用途 - ==React DOM用於將component render到瀏覽器的DOM上==

+ `<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>`
	+ `Babel`的library，`Babel`是一種編譯器，用於將一些現代的JavaScript以及JSX語法轉換為瀏覽器可使用的JavaScript語法

+ `<script type="text/babel" src="app.js"></script>`
	+ 載入我們使用React的JavaScript Code
	+ 透過在attribute宣告type為`text/babel`，來讓瀏覽器知道這個部分需要被Babel處理，所以就會透過前面引入的Babel來處理這部分

+ `crossorigin` keyword 
	+ 啟用對於其他`CORS`的支援
	+ value
		+ anonymous
		+ use-credentials
		+ 在默認狀態下(上方程式碼)，所給定的值會是`anonymous`
	+ 在沒有`crossorigin`的狀況下，其實還是能夠正常的存取這些資源，只是在執行script出現錯誤時，可能就只會出現==Script error==這樣簡短的錯誤訊息。

+ 為何在babel面前沒有加入`crossorigin`?
	+ 加入`crossorigin`的重點如前面提到的，==在錯誤時會出現詳細資訊==。而對於像babel作為編譯器的角色，並不是在我們的功能中占有核心的部分。
	+ `Babel`的錯誤通常會發生在轉換階段，會在console中顯示，不需要特別做額外處理
	+ 加上`crossorigin` keyword後，會需要瀏覽器去進行額外檢查，會略微降低效能