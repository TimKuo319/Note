#Vue #Frontend 

簡單的說，`<Script setup>`是`<Script> 加上 setup`的語法糖。

### script setup 的優點

>1. More succinct code with less boilerplate
>2. Ability to declare props and emitted events using pure TypeScript
>3. Better runtime performance (the template is compiled into a render function in the same scope, without an intermediate proxy)
>4. Better IDE type-inference performance (less work for the language server to extract types from code)

簡單來說，`<script setup>` 在解析的時候速度會比較快，而且也能夠讓 code 看起來更加簡潔

### Code Example

`<script setup>`
```vue
<template>
  <div>
    <p>{{ count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const count = ref(0)
const increment = () => {
  count.value++
}
</script>

```

`<script>`
```vue
<template>
  <div>
    <p>{{ count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  setup() {
    const count = ref(0)
    const increment = () => {
      count.value++
    }
    return { count, increment }
  }
}
</script>

```

在[Vue SFC](<./Vue SFC>) 中有提到，一個 SFC 中會包含一個元件的`template` `script` `style` 等部分，但我們既然在 `script` 中寫了邏輯，就需要將他 expose 出來給 template 中進行使用。

而傳統的方式就是在 `<script>` 中加上 `setup` function 來 expose 這些變數或 function。透過 `<script setup>` ，可以減去手動像上面一樣 return 的作法，讓程式碼可以更加精簡

### When to use script

- 具名輸出 (Named Export)
- 宣告其他 Options，如：`name`、`inheritAttrs`或其他插件需要的指定 Option
- 只要執行一次的 Side Effect
    - 一般 `<script>` 會在初次載入元件時執行一次，但每次載入元件都會呼叫 `setup()` hook
    - 因為 `<script setup>` 將所有邏輯都放到 `setup()` 內，所有每次渲染時都會執行所有程式碼，只想執行一次的程式碼要放在 `<script>`

## Reference

+ https://ithelp.ithome.com.tw/articles/10296330
