---
date: 2024-01-27 Sat 17:44
---
---


單純靠`React-hook-form`我們當然也能夠達到進行表單驗證的效果，我們可以透過從`useForm()`中取得`formState`物件，再取得裡面`errors`的屬性來顯示錯誤。

```tsx
import { useForm } from 'react-hook-form'

//...

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

let {register, handleSubmit, formState:{ errors }} = useForm<FormType>({resovler:zodResolver{schema}});

return (
	<form>
		<label>Age</label>
		<input {...register('age',{ required:true, minLength: 3})}/>
		{erros.name === 'age'  && <p>{erros.name.message}</p>}
	</form>
)
```

hookresovler的用途是用解析zod的schema，並追蹤我們form的狀態。從上面的程式碼可以看到我們先透過`z.object`，利用object的方式來建立驗證規則。利用zod的另外一個好處就是我們可以直接透過`z.infer`去推測我們form的type 不用像過去依樣