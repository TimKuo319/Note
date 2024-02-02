React.js



dom vs virtural dom

how boostrap loaded into react 

vite

由於component一次只能回傳一個html element，若是想要一個以上的element，可以用fragment包起來
<>
	...
	...
<>

useState -> 回傳兩個function

key -> 用來辨識單一元素


css-in-js

透過styled-components建立的物件，就算指定interface，也會繼承原生的js prop，像是onClick等等，我們只需在interface中新增我們需要的屬性就好

UI library
bootstrap material UI tailwindCSS daisyUI ChakraUI

strict mode

淺拷貝 深拷貝

react dev tool

children是React的特殊屬性，可以存取tag中的內容，像是<Text>abc</Text>，則可以取得abc這個內容。


-------------------------------------------------------------------------------------
forms

useRef 參考 HTMLInputElement
利用ref.current取得html屬性 ref.current.value可取得值
參考方式-> 在inputelement中加入ref屬性


或 useStae 也可以 -> 但每次state改變的時候會造成page reload，但有可能成為效能瓶頸
在利用這樣的狀況下，dom因為會依value去改變行為，但我們同時也用了state來取值，這樣可能導致狀態不一致，所以可以先在input element的value屬性加上{state}來確保狀態一致。


利用react-hook-form
透過register function取得input element的值，再透過react-form的handlesubmit將資料取出來。


html form 
name作為input element傳遞到後端的變數名稱