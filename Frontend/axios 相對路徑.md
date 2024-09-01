#Frontend 

```js
await axios.put(`/events/${googleEventId}`, requestData, {
        headers: {
          'Content-Type': 'application/json',
          Authorization: `Bearer ${localStorage.getItem('authToken')}`
        }
      })
```

當我們像以上程式碼，透過相對路徑去發 api 請求的時候。瀏覽器會幫我們補全網址讓我們的請求能夠正常的被送出。補全的方式是將`當前頁面的 protocol、domain、port`等等，放到路徑前面。

假設我現在在`https://localhost:5173/`的頁面發出以上請求。請求的路徑就會變成`https://localhost:5173/events`

