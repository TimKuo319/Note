
## Code

```js
navLinks.forEach(link => {  
    link.addEventListener('click', function (event) {  
        event.preventDefault();  
  
        const category = link.getAttribute('data-category');  
        const newUrl = `index.html/${category}`;  
        history.pushState({category}, '', newUrl); // Update URL without reloading the page  
  
        fetchProducts(category);  
    });  
});  
  
// Handle browser navigation (back/forward buttons)  
window.addEventListener('popstate', function (event) {  
    const category = event.state ? event.state.category : null;  
    if (category) {  
        fetchProducts(category);  
    } else {  
        fetchProducts();  
    }  
});
```

+ `history.pushState(state, title, url)` - 主要有三個參數
	+ state - 要傳入的狀態
	+ title - 頁面的新標題，通常為`''`
	+ url - 要替換的`新url`地址 


### pushState

 pushState是用來記錄瀏覽器狀態的，以上面的例子來說，在link被點擊之後，我們準備要接收新的資料重新渲染頁面。而在這之前，我們會先將`category`狀態儲存下來，也就是上面`history.pushState({category}, '', newUrl)`的方式，將狀態傳入瀏覽器的歷史中，在未來處理。後面的newUrl則是將當前網址列替換成新的Url，最後才是進行新資料的fetch動作。


### popState

透過去抓先前紀錄中的state狀態，來回復到上一個state。最常見的就是在按上一頁的時候，透過去找上一頁儲存的歷史狀態，來進行回復。