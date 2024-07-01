

## jQuery

+ 以`$`開頭，代表Jquery

### Manipulating element using jQuery

+ 存取html元素
	+ $('#element').html - getter
	+ $('element').html(`htmlElement`)  - setter
+ 取得form input內容
	+ $(selector).val()

### Adding New Elements to the DOM

+ append - 新增到DOM最後端
+ prepend -  新增到DOM最前端

### Event Handling

+ on - 可以同時給予兩個以上的event來幫助減少重複的程式碼
```js
$(selector).on('click keypress', callback)
```

### Event Object

+ 透過event listener時，通常會傳遞一個參數給後方的callback

### pseudo Class

+ 可以選擇具有pseduo class的屬性
```js
const $odd = $('a:odd');
```

### Change Element Property

+ $(selector).attr() - getter 
+ $(selector).attr('attr', 'property you want') - setter


### Modifyig style and Classes

+ element.css - getter
+ element.css('style', 'style you want') - setter

替selector新增class
+ element.addClass


### prevent Browser's Default Behavior

+ event.preventDefault()
	+ 預防一些瀏覽器預設行為
	+ 點擊連結時，自動開啟分頁
	+ 點擊下載連結時，自動下載等


### jQuery iteration

```js
collection.each(function(index, element){
	//do something with each element
})
```

### Unobtrusive JavaScript

+ Separation of concerns
+ Cross browser consistency
+ Progressive Enhancement