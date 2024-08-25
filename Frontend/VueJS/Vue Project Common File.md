
### vite.config.js

+ vite 設定檔
+ 可以進行路徑的別名設定

### App.vue

+ 通常只有一個，作為網頁的 `root component`

### main.js

```js
import './assets/main.css'

import { createApp } from 'vue'
import App from './App.vue'
import router from './router'

const app = createApp(App)

app.use(router)

app.mount('#app')

```

在 `main.js` 中，透過去引用 `App.vue` ，來創建一個 Vue instance，在透過 mount 將這個 instance 綁定到 html 中 id 為app 的元素。

### public folder

- **裡面的東西不會經過編譯和打包**，所以放在 📂 public 內的東西**不能被 Javascript 引用**
- 通常會放置 ( 不被 JavaScript ) 引用的靜態資源，例如：網頁標題欄 icon，即資料夾中的 `favicon.ico`
- 在整份專案進行打包的時候，裡面的檔案會直接被複製一份，**放在根目錄內**，所以要引用  public 內的資源時，要使用根目錄絕對路徑，舉例：要拿 `public/favicon.ico` 應寫成 `/favicon.ico`


### Package

+ 因為 browser 不會知道怎麼解析 Vue 的語法，所以要透過 package 來幫我們將這些語法轉換為 browser 看得懂的語言
	+ 有一點 `compile` 的感覺
	
	
+ 壓縮成較少份檔案，減少 request 次數

+ Minify -> 減少瀏覽器 parse 的時間

+ Uglify -> 將 js 都寫在一起，讓 code 變得不直觀

+ 原生 js 是沒有辦法支援 `import 圖片的` ，主要是透過 build tool 對原本的 source code 進行 package 後，幫忙處理引用的這件事情


## Reference

+ https://ithelp.ithome.com.tw/articles/10293243