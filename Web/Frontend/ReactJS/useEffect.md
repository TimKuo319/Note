---
date: 2024-01-28 17:40 Sun
---
---
## useEffect 

在React中，component是回傳一段JSX的function，而我們希望component會是一個`pure function`。`pure function`指的是一個函式在被傳入相同值的狀況下，都應該要得到相同的結果。

而在React中，會導致component不是`pure function`的程式碼就會被稱作`side effect`，這些`side Effect`通常跟component的render沒有關係，可能是手動修改DOM,或者連線取得資料等等。但又有可能影響到component每一次的回傳值。而這就是`useEffect`的作用，他就是用來處理這些`side Effect`的。

```tsx
useEffect(() => {
	axios.get('...')
	.then((res) => {'...'})
	.catch((err) => {'...'})
},[])
```

`useEffect`的第一個參數會接受一個callback function，裡面就會執行像前面提到的`side Effect`，第二個參數則是與這個`useEffect`綁定的參數，，當相關的變數發生變動，`useEffect`就會再執行一次callback function。

### useEffect的執行時機

當沒有提供第二個參數給`useEffect`的時候，`useEffect`預設會在每次component==被redner完後執行==，而上面的`useEffect`範例，只是給了一個空陣列作為第二個參數，在這樣的狀況下，`useEffect`只會在第一次component被render完後執行。也可以避免 無限迴圈。


### Clean up 

Clean up function在useEffect的作用通常會作為回傳值放在最後，目的是去還原在useEffect中的行為。

```tsx
useEffect(() => {

    const controller = new AbortController();
    const fetchData = async () => {
      setLoading(true);
      try{
      
        const res = await axios.get('https://jsonplaceholder.typicode.com/posts',{signal: controller.signal});

        setUsers(res.data)

        setLoading(false);
      }
      catch(err){
        if(err instanceof CanceledError) return;
        setError((err as AxiosError).message);
        setLoading(false);
      }    
    }
    
    fetchData();

    return () => {controller.abort()};

  },[])
```

像上面的例子中，我們利用`axios`來取得使用者相關的資料，但當使用者離開這個網頁，我們不需要再花費額外流量進行連線的時候，就透過`AbortController`來幫我們中斷連線。而最後的這個`controller.abort()`就稱為`clean up function`。原先在==useEffect中是進行連線==，所以最後退出useEffect的時候就要==還原(取消)連線==。

