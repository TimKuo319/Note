
## Get

```js
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok ' + response.statusText);
    }
    return response.json(); // 將響應解析為 JSON
  })
  .then(data => {
    console.log(data); // 在這裡處理獲取的數據
  })
  .catch(error => {
    console.error('There was a problem with the fetch operation:', error);
  });

```

## Post

```js
fetch('/api/1.0/user/signin', {  
    method: 'POST',  
    headers: {  
        'Content-Type': 'application/json'  
    },  
    body: JSON.stringify({  
        provider: "facebook",  
        access_token: accessToken  
    })  
})  
    .then(response => response.json())  
    .then(data => {  
        console.log("Response", data)  
    })  
    .catch(error => {  
        console.error("Error", error);  
    })
```

