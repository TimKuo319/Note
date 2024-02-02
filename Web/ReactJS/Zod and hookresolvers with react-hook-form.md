---
date: 2024-01-27 Sat 17:44
update: 2024-02-01 Thu 21:54
---
---


單純靠`React-hook-form`我們當然也能夠達到進行表單驗證的效果，我們可以透過從`useForm()`中取得`formState`物件，再取得裡面`errors`的屬性來顯示錯誤。

```tsx
import { useForm } from 'react-hook-form'

//...

interface FormType{
	age: number;
}

let {register, handleSubmit, formState:{ errors }} = useForm<FormType>({);
return (
	<form>
		<label>Age</label>
		<input {...register('age',{ required:true, minLength: 3})}/>
		{erros.name?.type === 'required'  && <p> The name field is required</p>}
		{erros.name?.type === 'minLength' && <p> The name field required at least 3 word</p>}
	</form>
)

```

但像上面這樣的方式，光是針對一個input element來說都有點冗長了，若是有很多個input element則很容易漏掉。所以我們可以利用一些第三方的`schema-based validation` library來幫助我們進行驗證。這邊使用的是`Zod`，


利用zod搭配上hookresolvers，改寫上方的程式碼。
```tsx
import { useForm } from 'react-hook-form'
import { z } from 'zod'
import {zodResolver} from '@hookform/resolvers/zod'

//....

const schema = z.object({
	age: z.number().min(18);
})

type FormType = z.infer<typeof schema>

let {register, handleSubmit, formState:{ errors, isValid } = useForm<FormType>({resovler:zodResolver(schema)});

return (
	<form>
		<label>Age</label>
		<input {...register('age',{ required:true, minLength: 3})}/>
		{erros.name === 'age'  && <p>{erros.name.message}</p>}
		<button isValid={isValid} type="submit" className="..."></button>
	</form>
)
```

hookresovler的用途是用解析zod的schema，並追蹤我們form的狀態。從上面的程式碼可以看到我們先透過`z.object`，利用object的方式來建立驗證規則。利用zod的另外一個好處就是我們可以直接透過`z.infer`去推測我們form的type 不用像之前需要再自己額外撰寫form的interface。最後再將這個schema透過`zodResolver()`傳入到form中即可完成。

整個表單裡面的內容差別是最多的，從第一段程式碼我們可以看到，`register`後面還需要加上屬性去進行驗證，但透過zod先定義好之後，`我們就不用在這個部分再手動新增驗證規則`。還有錯誤訊息的部分，當表單沒有通過驗證的時候，原先第一段程式碼也需要打兩行，依照不同的error type去輸出不同的錯誤訊息，可是這裡我們只需要確定error來源的對象，後面就可以動態的去輸出錯誤訊息，zod會幫我們輸出對應的錯誤訊息。相較第一段程式碼比起來差別就想當的大。

在最後button的部分，還可以透過`isValid`屬性來搭配`useForm'的`的`formState`中提供的`isValid`來進行表單輸入格式的驗證，只有當`isValid`屬性為true，按鈕才會處在一個可以按下的狀態。