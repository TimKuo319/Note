#Frontend #Vue 

### What is Routing

在 backend 中，我們通常會去定義 api endpoint，當使用者輸入對應的 URL 時，就會進到我們的 api ，照著我們撰寫的邏輯進行。

而前端同樣也有 routing，在現代常見的 library 像是 `react` 、`vue` 中，都只有一個主要的 app component，透過組合 component，來讓畫面有豐富的變化。

前端的 routing 主要就是在 `URL 發生變化的時後，依照輸入的 URL 來決定要怎麼呈現畫面給使用者`，透過 SPA 的方式，在收到不同的 URL 時對畫面進行部分更新，不會像傳統的 `SSR` 一樣，在 URL 有變化的時候， 直接回傳不同的 html 頁面，加重了瀏覽器 render 的 loading。


### Vue-Router 簡易使用

在使用 `vue-router` 的時候，我們要先定義好 `route rule` ，設定好`不同的 URL 應該要對應到哪些 component` 。

#### 設定 route rule 

從程式碼中可以看到，我們還可以在跳轉前進行檢查，也就是 `router.beforeEach` 的部分。如果直接呼叫 `next()` 就會將請求導向到 `to` 的 URL，或者是像下面在驗證失敗的時候，透過 `next('login')` 導向 `login` 頁面

```js
//index.js

import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'
import LoginView from '../views/LoginView.vue'

const routes = [
  { path: '/', name: 'home', component: HomeView },
  { path: '/login', name: 'login', component: LoginView }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

router.beforeEach((to, from, next) => {
  const isAuthenticated = localStorage.getItem('isAuthenticated')
  if(to.meta.requiresAuth && !isAuthenticated){
    next('/login')
  } else {
    next();
  }
})

export default router

```

#### 在 main.js 中設定使用 router 

```js
// main.js
import { createApp } from 'vue'

import App from './App.vue'
import router from './router'

const app = createApp(App)

app.use(router)

app.mount('#app')

```

#### 在 app.vue 中使用 router

在使用上，連結通常會使用 `router-link` 並設定 `to` 屬性，來告知 vue 這個連結要去哪一個 URL，當 `router-link` 被點擊的時候，下面的 `router-view` 就會依照當前的 URL 去先前設定好的 `route rule` 中找尋對應的 URL，然後 render 出那個 component。

>[!info] route-link 與 router-view 的關係
雖然 router-link 跟 route-view 通常都會一起出現，但兩者之間並不是綁定的關係。router-view 在進行變化的時候，是根據當前的 URL 的，也就是說，==就算沒有 router-link 的協助，只要 URL 有變化，router-view 同樣會改變。==最間單的例子就是不用 router-link 的狀況下，我們手動去修改瀏覽器的網址列，這樣同樣也能發現 router-view 有作用

```vue
// app.vue
<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div>
    <header>
      <nav>
        <ul>
          <li><router-link to="/">Home</router-link></li>
          <li><router-link to="/login">Login</router-link></li>
        </ul>
      </nav>
    </header>
    <main>
      <router-view />
    </main>
  </div>
</template>

<style scoped></style>

```


### Router 中的寫法差異 (to learn)

arrow function vs 直接指定 component

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