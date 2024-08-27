#Frontend #Vue 

### What is Routing

在 backend 中，我們通常會去定義 api endpoint，當使用者輸入對應的 URL 時，就會進到我們的 api ，照著我們撰寫的邏輯進行。

而前端同樣也有 routing，在現代常見的 library 像是 `react` 、`vue` 中，都只有一個主要的 app component，透過組合 component，來讓畫面有豐富的變化。

前端的 routing 主要就是在 `URL 發生變化的時後，依照輸入的 URL 來決定要怎麼呈現畫面給使用者`，透過 SPA 的方式，在收到不同的 URL 時對畫面進行部分更新，不會像傳統的 `SSR` 一樣，在 URL 有變化的時候， 直接回傳不同的 html 頁面，加重了瀏覽器 render 的 loading。