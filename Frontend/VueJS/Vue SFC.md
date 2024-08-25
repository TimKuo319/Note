#Vue  #Frontend

### What is SFC
SFC 是 Single-File Component，也就是 Vue Project 中的 `.vue` 檔案。

一個 SFC 中可以封裝一個 component 的 

- 邏輯 (JavaScript /`<script>`)
- 模板 (html /`<template>`)
- 樣式 (CSS / `<style>`)

```vue
<template>
  <div class="greeting">
    <h1>{{ message }}</h1>
    <button @click="changeMessage">Change Message</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello, Vue!'
    }
  },
  methods: {
    changeMessage() {
      this.message = 'Message changed!'
    }
  }
}
</script>

<style scoped>
.greeting {
  font-family: Arial, sans-serif;
  text-align: center;
}
h1 {
  color: #42b983;
}
button {
  padding: 5px 10px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
```

可以看到這裡包含了 `template` 來定義 component 渲染出來的樣子。`script` 用來表示 component 會用到的邏輯，`style` 則是定義 component 的樣式。

### Language Block 

- `Template` 限制：
    - 每個 .vue 文件只能有一個頂層 `<template>` 塊。
    - template 中必須有一個根元素（在 Vue 3 中這個限制被放寬，可以有多個根元素）。
- `Script` 限制：
    - 每個 .vue 文件只能有一個 `<script>` 塊（不包括 `<script setup>`）。
    - 如果使用 `<script setup>`，則不能再使用普通的 `<script>` 塊。
- `Style` 限制：
    - 可以有多個 `<style>` 塊，但建議只使用一個。
    - 使用 `scoped` 屬性時，樣式只會應用到當前組件的元素。


## Reference
+ https://ithelp.ithome.com.tw/articles/10295418 
 

### To learn 
+ `<script setup>` 
+ how `.vue` is parsed
