
### Props

父元件和子元件之間溝通的物件(?)，將父元件的狀態或資料`由上而下、單向傳遞給子元件`，這樣當父元件的狀態或資料更新後，更新的資料會向下`流向`子元件中。

### How child component get props

Parent.vue
```vue
<template>
  <div>
    <ChildComponent 
      :message="parentMessage"
      @custom-event="handleEvent"
    />
    <p>{{ receivedMessage }}</p>
  </div>
</template>

<script>
import { ref } from 'vue'
import ChildComponent from './ChildComponent.vue'

export default {
  components: { ChildComponent },
  setup() {
    const parentMessage = ref('Message from parent')
    const receivedMessage = ref('')

    const handleEvent = (msg) => {
      receivedMessage.value = msg
    }

    return { parentMessage, receivedMessage, handleEvent }
  }
}
</script>
```

child.vue
```vue
<template>
  <div>
    <p>{{ message }}</p>
    <button @click="sendEvent">Send Event</button>
  </div>
</template>

<script>
export default {
  props: ['message'],
  setup(props, { emit }) {
    const sendEvent = () => {
      emit('custom-event', 'Hello from child')
    }

    return { sendEvent }
  }
}
</script>
```

在上面的例子中，parent component 透過 `:message` 來傳遞 props 給 child component，而 child 是怎麼接收的呢？就是透過在 `setup` 中的參數宣告。

`setup` 其實可以接收兩個參數，一個是 `props`，第二個參數 `context` 是一個包含 function 的物件，可以供 child component 做使用，以上面來說，child component 就是解構 `context` 中的 `emit` ，用來發送事件給 parent component，具體的細節可以參考官方文件 [Composition API: setup() | Vue.js](https://vuejs.org/api/composition-api-setup.html#composition-api-setup)

## Reference

+ [Day 17: 元件溝通的原則 feat. props & emit - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10302898)

+ [Composition API: setup() | Vue.js](https://vuejs.org/api/composition-api-setup.html#composition-api-setup)


### to learn 


```vue
<script>
export default {
  props: ['message'],
  setup(props, { emit }) {
    const sendEvent = () => {
      emit('custom-event', 'Hello from child')
    }

    return { sendEvent }
  }
}
</script>
```

以上面的程式碼來說，props 的綁定在 vue 底層是如何執行的。

