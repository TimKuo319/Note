
### Difference
#### 功能差異
+ 管理功能程式碼的方式
+ `Composables`(?) 

#### 特性
+ `Options API` 允許依照選項類型來組合程式碼
	+ 有既定規則
	+ 按照選項管理
	 
+ `Composition API` 提供更大的靈活性，適合在大型專案中拆分 component
	+ 通常按照`功能邏輯`管理

#### 學習曲線
+ `Options API` 較為直觀
+ `Composition API` 較為彈性，但需要理解 reactive programming 的概念，學習曲線較高

### Options API
將 Vue 的程式碼依照特性的不同，分為 `data` 、`method` 、`computed` 等不同的的選項，讓開發者依照宣告 `Option` 的方式，來定義 component 中的邏輯

常見 Option :
+ `data` 
	+ 宣告資料的初始值
+ `method`
	+ 宣告 component 中能使用的方法
+ `computed`
	+ 回傳對資料的`加工或計算結果`

```js 
export default {
  data() {
    return {
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++
    }
  },
  computed: {
    doubleCount() {
      return this.count * 2
    }
  },
  mounted() {
    console.log('Component mounted')
  }
}
```

### Composition API 
Vu3 中引入的新 API，提供更靈活的方式來組織 component，在撰寫上不用定義 option ，有點

```js
import { ref, computed, onMounted } from 'vue'

export default {
  setup() {
    const count = ref(0)
    const doubleCount = computed(() => count.value * 2)

    function increment() {
      count.value++
    }

    onMounted(() => {
      console.log('Component mounted')
    })

    return {
      count,
      doubleCount,
      increment
    }
  }
}
```

