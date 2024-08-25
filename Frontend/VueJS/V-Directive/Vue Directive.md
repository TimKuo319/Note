
### What is V-Directive

在 Vue template 中，常見會使用到的 `v-` 開頭的指令就是 `v-directive` 。像是 `v-if`、`v-for`、`v-bind`。

### V-Directive strucutre

![[v-directive.png]]

- **Name**: 指令名稱
- **Argument**: 部份指令需要接收參數，接在`:`之後
    - 可以使用動態參數，如：`:[動態參數]`，但值應為**一般的字串**或是 `null` (表示移除綁定)
    - 動態參數不接受**單引號**或**空白**，這兩個字符本身就不是 HTML 接收的屬性名稱
- **Modifiers**: 部份指令提供修飾符，可以利用修飾符調整指令操作
- **Value**: 部份指令需要接收值，期待的值多數為 Javascript 表達式

以上面的例子來說，可以拆解成幾個部分。

`v-on` : 監聽某個事件，以上面來說就是監聽 `submit` 事件
`.prevent` : 呼叫 `event.preventDefault()` ，阻止預設提交
`onSubmit`: value 的欄位，當 submit 時要執行的事情

總合起來就是。==在收到 submit 時，阻止預設提交行為，並執行 onSubmit 這個 callback ==


## Reference

https://ithelp.ithome.com.tw/articles/10297034
