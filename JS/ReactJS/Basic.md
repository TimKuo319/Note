---
date: 2024-01-13 Sat 12:19
---
---

## Component

在React中，component是一種UI元件，範圍小至一個button，大至一個page都可以是component。而component就是一種回傳markup的js function。
```js
function MyButton() {  

return (  
	<button>I'm a button</button>  
);  
}
```

```js
export default function MyApp() {  

return (  

	<div>  
		<h1>Welcome to my app</h1>  
		<MyButton />  
	</div>  

);  
}
```

上面的`MyButton`就是一種component，而且要記得component的命名`必須以大寫為開頭`，這樣才可以讓react知道這是一個component。而html tag則必須開頭小寫。


## useState

	name convention{something , setSomething}
hooks
	custom hooks
	restrict
		only called on top of component(or other hooks)
props
