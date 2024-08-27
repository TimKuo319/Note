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

watch vs watchEffect