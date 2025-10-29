
---

首先初始化Node環境
```bash
npm init -y
```

```bash
npm install --save-dev typescript @types/express @types/node
```

`--save-dev`的參數使套件只會在開發過程中被用到，並不會被加入正式環境中。可以用來減少正式環境上套件的複雜度。因為typescript最終都會被編譯為javascript檔案，所以在正式環境上是可以不需要以上套件的。而後方`@types/expres`及`@types/node`則是express及node的型別檢查套件，在安裝後就可以讓編輯器偵測到，用以完成autocomplete或相關module的type checking。

```bash
npm install --save express
```
安裝express

```bash
npm install -g typescript
```
安裝typescript

```bash
tsc --init
```
在當前目錄下產生tsconfig.json檔，該檔案負責typescript編譯的相關設定。

```bash
npm install nodemon ts-node
```
ts-node是用來動態編譯ts檔案的套件，他會在記憶體中即時編譯並執行typescript程式碼，因此不會產生編譯後的js檔案。

## 在package.json中新增script

新增script利用nodemon來執行程式
```json
{ 
	//...,
	//...,
	scripts: {
		"test": "......",
		"start": "nodemon <your app entry-point>"
	}
}
```
## Unknown file extension .ts

起因 : 在package.json中設定了`type: "module"`。
解決方式 : 修改tsconfig.json來同步使得ts中的程式碼也符合ESM(ECMAScript)語法規範，做法如下:

```json
{
	"ts-node":{
		"esm": true
	},
	"compilerOptions":{
		...
		...
		"target":...
	},
	"include": ....
}
```

藉由在ts-node中加入esm flag使得ts語法規範符合esm。


參考:
[【 Node.js 】如何在 Node.js 中建立 TypeScript 的環境 - Jimmy 的架站筆記 (jimmyswebnote.com)](https://jimmyswebnote.com/create-nodejs-project-with-typescript/)[(15) How to Setup Node.js with TypeScript in 2023 - YouTube](https://www.youtube.com/watch?v=H91aqUHn8sE)
