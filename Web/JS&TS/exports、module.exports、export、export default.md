---
date: 2024-01-25 Thu
---
# Outline 

+ [exports、module.exports](<##exports、module.exports>)
+ [export、export default](<##export、export default>)
+ [Reference](##Reference)
## exports、module.exports

`exports`以及`module.exports`是`CommonJS語法`，他是reference到`module.exports`，所以透過`exports`，我們就能直接將variable、function、class導出做使用。而`module.exports`則是直接顯式的去導出。差別如下

```js
exports.fn = function(){
	//...
}

exports.a = "hello"
```

```js
fn = function(){
	//...
}

let a = "hello";

module.exports = { fn , a}
```

若是在一個檔案中選擇利用`exports`的方式去導出的話，則整個檔案應該要統一利用相同方式。就像前面所提到的，因為`exports`是去reference`module.exports`，所以如果在程式碼上方有`exports`，下方又利用`module.exports`導出的話，最後只會保留`module.exports`的結果，因為前面的`exports`被覆蓋掉了。

`module.exports`這邊也一樣，整個module在導出的時候是依賴`module.exports`這個屬性，所以要導出多個變數或function的話，就要像上面一樣利用中括號將要export的傳進來。若是再宣告一次`module.exports`則會去覆蓋掉第一次的`module.exports`。

## export、export default

`export`及`export default`是`ES6`語法，兩者都可用來導出變數、class、function等等，差別在與導入的時候。

```js
//export.js
export conat a = '100';

export const dogSay = fuction(){
	//..
}

export const CatSay = fuction(){
	//..
}

cosnt m = 100;
export default m;
```

```js
//index.js
import m from './export.js'
import { dogSay, CatSay } from './export.js'

console.log(m);
dogSay();
CatSay();

```

上面可以看到，利用`export`的方式，想要取得對應的function就必須要利用{}解構來指定名稱取得，而以`export default`的話則不需要，甚至上面的m其實也能夠改成其他名字，在log出來的時候也一樣能夠得到值為100。

## Reference

[module.exports – How to Export in Node.js and JavaScript (freecodecamp.org)](https://www.freecodecamp.org/news/module-exports-how-to-export-in-node-js-and-javascript/)
[javascript - exports、module.exports和export、export default到底是咋回事 - JS那些事儿 - SegmentFault 思否](https://segmentfault.com/a/1190000010426778)