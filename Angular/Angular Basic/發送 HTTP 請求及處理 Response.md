
## Intro

要作為一個可用的前端應用程式，必然會少不了與後端溝通的過程，而這部分就是透過 API 來進行溝通。


## HttpClient 

HttpClient 是 angular 提供用來進行 http 請求的介面，有許多種配置，像是`withInterceptors`、`withJsonSupport`... 等，可以參考官方文件 [Setting up HttpClient • Angular](https://angular.dev/guide/http/setup)。

HttpClient 中通常會在 `app.config.js` 中做設定，程式碼如下，設定完成後即可在要使用的地方(通常會是`service`，因為會將呼叫 API 的邏輯統一集中在 service 中)。

```ts
export const appConfig: ApplicationConfig = {
  providers: [provideHttpClient()],
};
```


>[!info]
> HttpClient 與我們自訂義撰寫的服務都可以透過 inject 的方式被注入使用。但為何 HttpClient 需要到 `app.config.ts` 中進行設定呢？
> 
> 在我們自訂義的 service 中，通常會在上方透過 `providedIn: root` 的方式將 service 自動註冊到 angular 的 IoC container 中，但 HttpClient 本身是更為複雜的服務，其中有許多不同的設定，因此用 providedIn 沒有辦法滿足這些配置，所以需要使用 provideHttpClient 的方式來做註冊。 

## 基本用法

http client 預設回傳 response body 的內容，可以在方法後面加上回傳值的型別增加可讀性及 IDE 的語法偵測。


- 可使用 `catchError()` operator 來進行轉換邏輯，提升在  `error()` 中的可讀性

```ts
  private fetchAvailablePlaces(url:string, errorMessage:string) {
    return this.httpClient.get<{places: Place[]}>(url)
    .pipe(
      map(resData=> resData.places),
      catchError((error) => {
        let errorMessage = error.message;
        return throwError(() => new Error(errorMessage));
      })
    )
  }


```


>[!info]
另外，http client 的 method 除了可以填入 url 作為基本的 API 呼叫之外，還具備第二個參數，`observe` 能夠用來觀察不同的情況，可以在需要獲得 response 更詳細的資訊時做使用
>- `observe`
>	- `response`
>		- 從 next 中會得到的 response 物件，包含 header、status code、body 等
>	- `event`
>		- 在 request - response lifecycle 中接收到任何 event 時就會觸發．

## Optimistic Update

Optimistic Update，樂觀更新，指的是在前端收到使用者資料時就先針對畫面進行更新，來讓使用者能夠立即得到回饋，增加使用者的體驗。

這樣的做法預設是與 backend 的請求會成功，但在錯誤時會導致前端與後端不一致 ex: 使用者以為資料已經新增，但實際失敗

所以要針對請求的錯誤狀況進行 rollback 處理，下方是一個簡單的程式碼例子，會在使用者點選畫面上的某些元素時加到另外一個區域。而下方同樣使用了樂觀更新的方式，立即的進行狀態的更新，但在得到錯誤請求時，就將狀態回到先前的狀態，來避免前後端不一致的情況。

```ts
  addPlaceToUserPlaces(place: Place) {
    const prevPlaces = this.userPlaces();

    if(!prevPlaces.some(p => p.id === place.id)) {
      this.userPlaces.update((prevPlaces) => [...prevPlaces, place]); 
    }

    return this.httpClient.put(`http://localhost:3000/user-places/`, { placeId: place.id })
    .pipe(
      catchError((error) => {
        this.userPlaces.set(prevPlaces);
        this.errorService.showError('Could not add place to user places.');
        throw new Error('Could not add place to user places.')
      })
    )
 
```

## 統一錯誤處理

可以將 errorMessage 做成一個 shared service，在發生錯誤時將訊息傳遞給 shared service 來達到統一錯誤處理
[Course: Angular - The Complete Guide (2025 Edition) | Udemy](https://www.udemy.com/course/the-complete-guide-to-angular-2/learn/lecture/44116360#overview)


## http client 的 query parameter、path parameter 使用方式

[Making requests • Angular](https://angular.dev/guide/http/making-requests#setting-url-parameters)



## 待補

- [ ] service 層除了處理 httpClient 之外，還會做哪些事

- [ ] interceptor 影片待補 - udemy angular 課程 12 節 238 

