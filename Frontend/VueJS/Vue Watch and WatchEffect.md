- **`ref`** 的主要目的是創建響應式數據，當數據變化時，自動更新 DOM。
- **`watch`** 的作用則是在 `ref`（或其他響應式數據）變化時，執行一些附加的邏輯操作，這些操作通常不是簡單的 DOM 更新，而是更複雜的 `sideEffect`。

+ 使用 watch 時，如果要監聽的 ref `是一個物件`，則需要使用函數型回傳值，其他則直接監聽`變數`即可。

+ 如果是 array 的 watch 還可增加 `deep` 選項 
	+ 在沒有設定 `deep` 選項的狀況下，watch 監控的只有 array 的 `長度變化` ，如果想要細緻到 array 中的某一個元素變動時，就呼叫 callback，則需要使用 `deep`

```js
const items = ref([{ id: 1, name: 'Item 1' }, { id: 2, name: 'Item 2' }]);

watch(items, (newItems) => {
  console.log('Items changed:', newItems);
}, { deep: true });

```


### to learn

##### Brief diff
`watch` 和 `watchEffect` 都是 Vue 3 中用於響應式監聽的函數，但它們有一些重要的區別：

1. 監聽方式：
    - `watch` 明確指定要監聽的響應式引用或 getter 函數。
    - `watchEffect` 自動追蹤其內部所使用的所有響應式依賴。
2. 執行時機：
    - `watch` 默認情況下只在監聽的值發生變化時才執行。
    - `watchEffect` 在組件初始化時立即執行一次，之後在其依賖變化時執行。
3. 參數：
    - `watch` 回調函數接收新值和舊值作為參數。
    - `watchEffect` 不提供這些參數。
4. 控制粒度：
    - `watch` 提供了更細粒度的控制，可以指定具體監聽哪些值。
    - `watchEffect` 自動收集依賴，更簡潔但控制較少。
5. 停止監聽：
    - 兩者都返回一個停止函數，但 `watchEffect` 的停止函數會在組件卸載時自動調用。
