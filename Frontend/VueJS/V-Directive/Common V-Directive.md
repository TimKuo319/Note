#Frontend #Vue 

#### v-Model

+ `v-model` 其實就是 `v-on` 及 `v-bind` 的組合


#### v-if、v-else

+ `v-if` 在值為 `true` 的時候會 render 出元素
+ `v-else` 是用來實現 `conditional render` 的方式，==但必須緊跟在 v-if ==，當前面的條件為 false 的時後，就會到套用 `v-else` 的元素進行 render

```js
<div v-if="conditionA">A</div>
<div v-else-if="conditionB">B</div>
<div v-else>C</div>
```

#### v-for

#### computed vs watch

## Reference

+ https://ithelp.ithome.com.tw/articles/10297907