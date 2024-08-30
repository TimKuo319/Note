
### main.js 設定

```js
import { createApp } from 'vue'
import App from './App.vue'
import ElementPlus from 'element-plus'
import vue3GoogleLogin from 'vue3-google-login'
import 'element-plus/dist/index.css'
import router from './router'

const app = createApp(App)

app.use(ElementPlus)

app.use(router)

app.use(vue3GoogleLogin, {
  clientId: 'your-client-id'
})

app.mount('#app')

```

根據官方文件 [vue3-google-login](https://devbaji.github.io/vue3-google-login/#installation)，在 `main.js` 中設定 `clientId`。 

google login 有提供不同種類的登入方式，像是 `AuthCode` `Token` `Credential` 等等。對於要使用到 google api 服務通常需要讓 user 透過 [OAuth](../../Network/OAuth) 的方式來授權應用程式可以存取資訊的範圍。

這個部分可以透過 google 提供的 `InitxxClient` 來指定 client 的授權範圍。大致上配置如下

```js
const login = () => {
  googleSdkLoaded((google) => {
    google.accounts.oauth2
      .initTokenClient({
        client_id: 'your-client-id',
        scope: 'email profile openid https://www.googleapis.com/auth/calendar',
        callback: (res) => {
          localStorage.setItem('authToken', res.access_token)
          router.push('/calendar')
        }
      })
      .requestAccessToken()
  })
}
</script>
```

從上面的程式碼可以看到，對於基本的個人資訊，可以用像是 `email` `profile` `openid` 等等簡單的關鍵字來授權，如果還需要授權其他服務的話，就要根據官方文件給出的 `endpoint` 去進行指定，以上面的例子來說就是`https://www.googleapis.com/auth/calendar`。最後再透過 `requestAccessToken` 來啟動整個請求。

## Reference

+ [Vue3 串接 Google OAuth 登入 【2022 最新】 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10304289) 

- [vue3-google-login](https://devbaji.github.io/vue3-google-login/#using-google-sdk)

- https://developers.google.com/identity/oauth2/web/reference/js-reference#google.accounts.oauth2.initTokenClient