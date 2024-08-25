#Frontend #Vue 

### What is Reactivity (響應式)

響應式指的是 Vue 幫助我們即時更新相應資料這件事情。

Vue 3 推出的 `reactive()` 以及 `ref()` 就是用來攔截資料的讀取跟寫入，所以需要響應式的資料需要 `reactive()` 以及 `ref()` 來處理。

`reactive` 是用 Proxy 實作，`ref` 是用 get 、set 來實作。


>[!info] Proxy
> Proxy 是 JS 的原生語法，翻譯為代理，只要透過 proxy 代理操作物件，==就能夠攔截所有對於物件的操作==


#### ref 使用

```js
import { ref } from 'vue'

const count = ref(0)
console.log(count.value) // 0
count.value++
console.log(count.value) // 1
```

+ `ref` 可以用於建立任何響應式數據
+ 將 primitive type 包在一個物件中，透過 `.vaule` 的方式訪問或修改值

#### reactive 使用

```js
import { reactive } from 'vue'

const state = reactive({ count: 0 })
console.log(state.count) // 0
state.count++
console.log(state.count) // 1
```

+ 主要用於 `object, array`
+ 直接返回一個 proxy 物件


| reactive | ref      |            |
| -------- | -------- | ---------- |
| 接收參數     | 物件型別     | 都可以        |
| 回傳       | Proxy 物件 | RefImpl 物件 |
| 取值       | 同普通物件    | `.value`   |




## Reference 
+ [真的好想離開 Vue 3 新手村 - Day 10: 從原生 JS 理解 Vue 3 響應式基礎 - reactive & ref (上) - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10298023)

+ [真的好想離開 Vue 3 新手村 - Day 11: 從原生 JS 理解 Vue 3 響應式基礎 - reactive & ref (下) - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10298545)


### To learn

+ JavaScript Proxy
+ Getter Setter
