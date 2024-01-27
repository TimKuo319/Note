---
date: 2024-01-27 Sat 17:44
---
---

# Outline

+ [Zod](##Zod)
+ [hookresolvers](##hookresolvers)

## Zod

單純靠`React-hook-form`我們當然也能夠達到進行表單驗證的效果，我們可以透過從`useForm()`中取得`formState`物件，再取得裡面`errors`的屬性來顯示錯誤。

```tsx
import { useForm } from 'react-hook-form'

return (
	<form>
		<label>Age</label>
		<input {...register('age',{ required:true, minLength: 3})}/>
		{erros.name?.type === 'required'  && <p> The name field is required</p>}
		{erros.name?.type === 'minLength' && <p> The name field required at least 3 word</p>}
	</form>
)

```

但像上面這樣的方式，光是針對一個input element來說都有點冗長了，若是有很多個input element則很容易漏掉。所以我們可以利用一些第三方的`schema-based validation` library來幫助我們進行驗證。這邊使用的是`Zod`

