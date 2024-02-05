---
date: 2024-02-05 Mon 16:15
---
---

linter的用途就是透過靜態分析我們的程式碼來幫助維護我們的程式碼品質，由於目前以JS/TS開發為主，所以是紀錄ESLint的使用方式。

+ [How to use ESLint in vscode](<##How to use ESLint in vscode>)
+ [Reference](##Reference)

## How to use ESLint in vscode
---
1. 安裝ESLint的vscode extension
2. 安裝eslint
```bash
npm install --save-dev eslint
```
安裝完成後就可以透過`npx eslint --init`來初始化專案的eslint設定，會出現一連串的問題，其中第一個問題是詢問要如何使用eslint，選擇`To check syntax and find problem`。接著就依照自己的專案需求做選擇。可以參考[VSCode中使用 Prettier及ESLint. 前言 | by HeroJia | Medium](https://medium.com/@HeroJia/%E5%9C%A8vscode%E4%B8%AD%E4%BD%BF%E7%94%A8-prettier-eslint-5b708cf83213)
3. 根據



## Reference
---

Official documentation
	[Getting Started with ESLint - ESLint - Pluggable JavaScript Linter](https://eslint.org/docs/latest/use/getting-started)
	
How to use eslint
	[How to use ESLint with TypeScript | Khalil Stemmler](https://khalilstemmler.com/blogs/typescript/eslint-for-typescript/)
	[VSCode中使用 Prettier及ESLint. 前言 | by HeroJia | Medium](https://medium.com/@HeroJia/%E5%9C%A8vscode%E4%B8%AD%E4%BD%BF%E7%94%A8-prettier-eslint-5b708cf83213)
Unused-import autofix
	[reactjs - How can I remove unused imports/declarations from the entire project of React Typescript? - Stack Overflow](https://stackoverflow.com/questions/64365300/how-can-i-remove-unused-imports-declarations-from-the-entire-project-of-react-ty)
	[eslint-plugin-unused-imports - npm (npmjs.com)](https://www.npmjs.com/package/eslint-plugin-unused-imports)

JSX support 
	['React' must be in scope when using JSX react/react-in-jsx-scope | bobbyhadz](https://bobbyhadz.com/blog/react-must-be-in-scope-when-using-jsx)