#Frontend #Vue 

### What is Routing

在 backend 中，我們通常會去定義 api endpoint，當使用者輸入對應的 URL 時，就會進到我們的 api ，照著我們撰寫的邏輯進行。

而前端同樣也有 routing，在現代常見的 library 像是 `react` 、`vue` 中，都只有一個主要的 app component，透過組合 component，來讓畫面有豐富的變化。

前端的 routing 主要就是在 `URL 發生變化的時後，依照輸入的 URL 來決定要怎麼呈現畫面給使用者`，透過 SPA 的方式，在收到不同的 URL 時對畫面進行部分更新，不會像傳統的 `SSR` 一樣，在 URL 有變化的時候， 直接回傳不同的 html 頁面，加重了瀏覽器 render 的 loading。


### Vue-Router 簡易使用

在使用 `vue-router` 的時候，我們要先定義好 `route rule` ，設定好`不同的 URL 應該要對應到哪些 component` 。



>[!info] route-link 與 router-view 的關係
雖然 router-link 跟 route-view 通常都會一起出現，但兩者之間並不是綁定的關係。router-view 在進行變化的時候，是根據當前的 URL 的，也就是說，==就算沒有 router-link 的協助，只要 URL 有變化，router-view 同樣會改變。==最間單的例子就是不用 router-link 的狀況下，我們手動去修改瀏覽器的網址列，這樣同樣也能發現 router-view 有作用




## To learn

- composition api 中 router 的使用。

- vue router 的模式選擇 
	- history 
	- hash

- Router 中的寫法差異 
	- arrow function vs 直接指定 component
```js
const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      name: 'home',
      component: HomeView
    },
    {
      path: '/about',
      name: 'about',
      // route level code-splitting
      // this generates a separate chunk (About.[hash].js) for this route
      // which is lazy-loaded when the route is visited.
      component: () => import('../views/AboutView.vue')
    }
  ]
})
```