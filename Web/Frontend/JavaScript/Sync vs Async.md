## Sync and Async

+ 同步(`Sync`)可以想像成程式照著順序執行，在single-thread的狀況下，如果前面有一個function或loop執行的比較久時，就會造成後面的程式碼需要等待的情況

+ 非同步(`Async`)的話則與上面的狀況不同，能夠同時間做`一件以上的事情`。
	+ 以JavaScript來說，我們的程式碼中可能會有loop、function，以及像是setTimeout等等的Web API。然而像是setTimeout這樣會delay的函式，我們其實是將他交給瀏覽器進行處理，先繼續執行我們的主程式。等到函式的結果在瀏覽器已經處理完了，他會再回傳給我們。

```JavaScript
console.log('Start');

setTimeout(() => {
  console.log('Inside setTimeout');
}, 2000);

console.log('End');

```

以上面的例子來說，整體的輸出會是
```bash
Start
End
Inside setTimeout
```

可以看到`console.log('End')`並沒有被SetTimout給block住，這就是`Async`的一種表現。

## Promise

+ Promise是用於非同步操作中的一個對象。代表的是一個非同步操作最終`成功`或`失敗`的結果。
	+ 三個狀態
	+ pending
	+ resolve
	+ reject

+ 可以想像成我們透過promise在進行回傳的時候，是在告訴編譯器或瀏覽器，`"這個操作是非同步的，他可能會在未來的某一個時間點完成，且可以透過then以及reject來處理它的結果"`

```js
function fetchData() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const success = true; // 可以是任何条件的判断
            if (success) {
                resolve('Data retrieved successfully');
            } else {
                reject('Failed to retrieve data');
            }
        }, 2000);
    });
}

// 使用 Promise 的函数
fetchData()
    .then(data => {
        console.log(data); // 输出: Data retrieved successfully
    })
    .catch(error => {
        console.error(error); // 如果操作失败，输出错误信息
    });

```
## async and await

+ async及await是用來簡化對promise處理的。當我們接收到一個promise的時候，會使用`then`以及`catch`來進行處理，當處理的狀況越多時，就會使的`then`、`catch`的使用變得相當冗長。async及await keyword出現，能讓這樣的處理變得更像同步的程式碼

+ `await`只能在`async`function內使用

+ 在需要使用到await的地方再加上async即可

可以看到，下面的方式直接用變數去接收`then`的回傳值，並將整段程式碼用`try catch`包起來，當reject時，就會跑到`catch`block中。
```js
async function fetchData() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const success = true;
            if (success) {
                resolve('Data retrieved successfully');
            } else {
                reject('Failed to retrieve data');
            }
        }, 2000);
    });
}

// 使用 async/await 的函数
async function main() {
    try {
        const data = await fetchData();
        console.log(data); // 输出: Data retrieved successfully
    } catch (error) {
        console.error(error); // 如果操作失败，输出错误信息
    }
}

main();

```

## Reference

[[JavaScript] 一次搞懂同步與非同步的一切：一次做幾件事情 — 同步(Sync)與非同步(Async) - itsems_frontend - Medium](https://medium.com/itsems-frontend/javascript-sync-async-22e75e1ca1dc)

[Async function / Await 深度介紹 | 卡斯伯 Blog - 前端，沒有極限](https://www.casper.tw/development/2020/10/16/async-await/)